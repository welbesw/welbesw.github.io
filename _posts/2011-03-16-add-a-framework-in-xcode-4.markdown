---
layout: post
title:  "Add a Framework in Xcode 4"
date:   2011-03-16 11:00:00
tags: Xcode iOS
---

In Xcode 4, there's no option click menu item to provide for adding a new framework to your project like there was in Xcode 3.  After hunting around in Xcode 4, I was able to figure out how to add a new framework in:

1. Click on the project in the navigator.
2. Click on the target in the project pane.
3. Click on the "Build Phases" tab.
4. Expand the "Link Binary With Libraries" group.
5. Click the "+" icon at the bottom to add in a new framework.
6. Select the desired framework from the list.

I'm sure I'll be back to this article the next time I can't remember how to add a new framework again :)