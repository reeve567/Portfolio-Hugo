+++
title = "Indie Game Library"
date = "2020-10-01T00:00:00-05:00"
author = "Reeve"
authorTwitter = "" #do not include @
cover = ""
tags = ["coding-project","kotlin"]
keywords = []
description = "Indie Game Library is a Kotlin application to keep track of games and their progress."
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

# Indie Game Library

I play a lot of different games. 267 on Steam, 161 on Epic Games, and 198 without an organizer. Of course, some of them I haven't gotten around to, and some have hardly any play time. But luckily, on Steam and Epic Games, I can keep track of my whole library easily. This isn't true for those indie games whose playerbase consists of a handful of players, though. These games have to either be on itch.io, or they are just hosted somewhere on the internet. That's where my little program comes in, sorting their downloads and information. I created this with one website in mind that I mainly go to, but there's masked download links and all that jazz, so here we are.

This project has gone through many iterations, but the main two versions are TornadoFX (JavaFX with Kotlin extensions), Dear ImGui (C++ -> Kotlin library). The original point was to organize my different games into an easily navigatable file structure, but then I was having fun and ended up with UI. The difficult part was to also include the different versions of games into the same folders, and to include the most common types of compression. In order to figure out what zip files were of the same game, I looked for the executable of the game and named it based off that (provided there wasn't a generic name). 

## TornadoFX

This one is a nice library for Kotlin using dependency injection and whatnot, but I struggled to get it to look exactly how I wanted.  This is the first version, and goes by [Organization](https://github.com/reeve567/Organization)
{{< figure src="/images/tornadoFxOrganization.png" alt="TornadoFX rendition" position="center" caption="TornadoFX rendition" captionPosition="center">}}

## Dear ImGui

This one isn't really for applications by themselves, rather to apply on top of games and whatnot, but I really like the look of the GUI for it.  This was the second one, which went through a decent amount of changes, and can be found at [ImOrganization](https://github.com/reeve567/ImOrganization)
{{< figure src="/images/dearImGuiOrganization.png" alt="Dear ImGui rendition" position="center" caption="Dear ImGui rendition" captionPosition="center">}}
