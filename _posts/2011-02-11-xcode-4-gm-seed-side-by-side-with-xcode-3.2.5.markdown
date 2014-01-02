---
layout: post
title:  "Xcode 4 GM seed side by side with Xcode 3.2.5"
date:   2011-02-11 11:00:00
tags: Xcode
---

Apple released a new version of Xcode 4 - the GM seed - that can now be used to develop and submit apps for the app store.  Previously, the previews that they had available could not be used for building apps for app store submission.  This made it somewhat useless for me at the time.  With the ability to build and submit apps to the app store, I decided it was time to install the Xcode 4 GM seed and start using it with real projects being built for enterprise and the app store. 

So on Monday, I installed Xcode 4 after downloading the almost 4GB install packaged from [developer.apple.com](http://developer.apple.com).  When installing, I just followed the wizard and allowed it to use default options.  What I didn't realize is that it would wind up upgrading the existing Xcode on my system and therefore require me to use just Xcode 4 for development.  I hadn't put too much thought about it in before hand and so I decided to go ahead with using Xcode 4 with my existing projects.  Xcode 4 really is different than 3, so I needed to learn a lot about where tools and interfaces were to be able to be effective.  It seems like its nice... I just need to get my brain wrapped around it all yet. 

Using Xcode 4 for everything was going fine until I was working with a project that includes three20.  (I've never really liked having dependencies on three20, but I've had to work with it on a couple projects)  Compiling a projects with three20 for debug worked just fine, but once I tried to create a distribution build, no dice.  The custom scripts that are included in three20 for the code signing process aren't working for Xcode 4.  Uggh.  Now I was up creek without a paddle... namely my trusted old friend Xcode 3.2.5.

After some research and pinging back and forth with [Aaron Douglas](http://astralbodies.net), I read the 3 page PDF document that Apple provides with the Xcode 4 distribution.  Sure enough... on the last page of the document it describes exactly how to install Xcode 4 side by side with Xcode 3.2.5, without upgrading the existing installed Xcode version.  So... I decided to start with a clean slate approach and uninstall Xcode 4 and then reinstall 3.2.5 so I could get back to a functioning version of the tool that I know works for my needs and then attempt to install Xcode 4 to be able to use that whenever appropriate.  To uninstall Xcode 4, I ran the following:

sudo /Developer/Library/uninstall-devtools –mode=all

After uninstalling Xcode 4, I rebooted as the uninstall process recommended and then proceeded to install 3.2.5 using the wizard.  After install, I confirmed that I had a functioning Xcode again and was able to build distribution build for a project that included three20.  (BTW: It appears that the three20 folks are working on updating their projects to work with Xcode 4 but I couldn't find an ETA for that and didn't want to invest time myself with it)

Next, I installed Xcode 4 in a side by side install with 3.2.5.  Per the document included with the distribution, when prompted for a directory to install to, I selected a directory other than the /Developer directory that I called /xcode4.  The action listed still said "upgrade", which made me a little hesitant, but after clicking Go to install, I was pleased to find that I now had Xcode 4 and Xcode 3.2.5 installed and functioning.  I also noticed that Xcode 4 did not mangle the project files in any way that prevented Xcode 3.2.5 from reading them.  Xcode 4 adds some additional files, but it seems like they play pretty nicely with each other.

Looking forward to more time and learning with Xcode 4 while still having 3.2.5 around to get things done as needed.