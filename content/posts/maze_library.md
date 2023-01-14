+++
title = "Maze Library"
date = "2023-01-13T20:30:04-05:00"
author = "Reeve"
authorTwitter = "" #do not include @
cover = ""
tags = ["coding-project", "kotlin", "library"]
keywords = ["library", "mazes"]
description = ""
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

# MazeLib

MazeLib is a library I had a blast working on, and I'm proud of how it turned out.  Included are several algorithms, and at the moment are all perfect mazes (no detached walls or loops).
As of now, there are four algorithms, and I plan on adding more in the future. These algorithms are all used for generating mazes, and some of them you can also generate solutions for.

- (G_) BinaryTree (North-Eastern bias, y = 0 & x = max - 1)(~25% dead ends)
- (G_) Sidewinder (Upwarard bias, y = 0)(~25% dead ends)
- (GS) RecursiveBacktracker (Unbiased, more memory needed [vs HuntAndKill])(~10% dead ends)
- (G_) HuntAndKill (Unbiased, more time needed [vs RecursiveBacktracker])(~10% dead ends)

A little visual demo is provided in the tests directory (java/dev.reeve.mazelib/GridTest). It uses RecursiveBacktrack by default, but it's trivial to switch to another.

## Dependency

If you'd like to use this library in a gradle project, you can find it in my [maven repository](https://repo.reeve.dev/#browse/browse:maven-releases:dev%2Freeve%2FMazeLib).

{{< code language="kotlin" >}}

repositories {
    maven("https://repo.reeve.dev/repository/maven-snapshots/")
}

dependencies {
    implementation("dev.reeve:MazeLib:1.0.5")
}

{{< /code >}}

## Sample Code

Here's some sample code to get a 2x2 going:

{{< code language="kotlin" >}}

val generator = RecursiveBacktrackGenerator(random = Random(567))

// Generating a 2x2 maze with all areas considered valid, all invalid would be position -> false
// This is useful if you want to have like a gap in the middle of your maze, which should be taken into consideration when generating the maze,
// to make sure that the maze is still solvable.
// val maze = generator.generateMaze(2, 2) { position -> true }
// This is the main way to generate a maze, but because `RecursiveBacktrackGenerator` is a `MazeGeneratorWithSolution`, you can also generate a solution.

// In this version of the `generateMaze` method, the second parameter is the entrance, and the third is the exit.
val result = generator.generateMaze(2, 2, MazePosition(0, 0), MazePosition(1, 1))

val maze = result.first
println(maze.points.joinToString("\n") { it.joinToString() })
// MazePoint{sides=[true, false, false, false], position=MazePosition{x=0, y=0}, updateOrder=0}, MazePoint{sides=[true, false, false, false], position=MazePosition{x=0, y=1}, updateOrder=3}
// MazePoint{sides=[false, true, true, false], position=MazePosition{x=1, y=0}, updateOrder=1}, MazePoint{sides=[false, false, true, true], position=MazePosition{x=1, y=1}, updateOrder=2}
// Sides are [EAST, SOUTH, WEST, NORTH] (there are convenience variables linked to their names)

val path = result.second
println(path.joinToString())
// MazePosition{x=0, y=0}, MazePosition{x=1, y=0}, MazePosition{x=1, y=1}
{{< /code >}}