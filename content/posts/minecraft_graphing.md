+++
title = "Minecraft Graphing"
date = "2021-03-22T00:00:00-05:00"
author = "Reeve"
authorTwitter = "" #do not include @
cover = ""
tags = ["coding-project", "kotlin", "minecraft", "spigot", "plugin"]
keywords = ["minecraft", "plugin", "graphing"]
description = "A minecraft plugin that allows players to graph functions in game."
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

# Minecraft Graphing

Although it was a short project, this one was one of my latest favorites because it was a lot of fun to play around with. This plugin allows you to create 3D graphs in-game using particles. The idea came after my Calculus III class one day, since I realized I could make almost any shape if I just chained them together. At present, the plugin only supports one graph at a time, but it'd be pretty trivial to add in more.
The version I made available on [Github](https://github.com/reeve567/MinecraftGraphing) is for Spigot 1.16.5, and it's pretty simple.

At the moment, it is only set up to be used as a standalone plugin, but it could be possible to make it into a library.  The graph function is set in the `config.json`, and has a default value of `x^2 + y^2`.

{{< figure src="/images/saddleFunction.png" caption="Here's a saddle function (x^2 - y^2 - z = 0) made from the 'suspended' particles. " >}}

{{< figure src="/images/saddleFunction2.png" caption="The same function with redstone particles instead." >}}