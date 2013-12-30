---
layout: post
title:  "iPhone Color Picker"
date:   2011-04-01 11:00:00
categories: iPhone iOS
---

At [Centare](http://www.centare.com), I've recently developed a color picker for iOS (iPhone and iPad).  It's a set of UIViews that allows a user to pick a color via a Hue, Saturation, Value (HSV) interface.  Here's a screenshot of what it looks like inside a test application.

![Color Picker Screenshot](/img/color_picker.png)

This was a great exercise in designing UIViews that interact via delegate methods.  The hue view at the bottom drives the hue color of the color palette at the top in real time as the hue is changed.  The saturation and value view generates the output pixel values and displays them on screen.  Both of the views report back the currently selected color information based on where the user is touching the screen.  Each of these views is then contained in a parent view that wraps all of the color picker functionality to make it easy to drop the color picker into a parent view.  There's a  delegate that provides the selected color as it changes, which is then used to update the currently selected color swatch shown on the left.

The red, green, and blue sliders shown in the above application are just for testing and allow changing the currently selected color by updating an input RGB value.  They also change based on the selected RGB value picked in the color picker.

I really enjoyed creating the logic for the color math based on the HSV and RGB models.  My first attempt worked great on the iPhone simulator, but was dog slow on the actual device.  I had to optimize the algorithm to be able to convert the colors and draw the palette in real time on the device.  It works great now and is very responsive to the user's touches.  

I've implemented this color picker into the [Bradley Color Spec app](http://itunes.apple.com/us/app/bradley-colorspec/id412479793?mt=8&ign-mpt=uo%3D4), a client of ours here at [Centare](http://www.centare.com).  You can [download](http://itunes.apple.com/us/app/bradley-colorspec/id412479793?mt=8&ign-mpt=uo%3D4) the app and take a look if you want to take it for a test run.  Also, if you're in the market for solid surface materials, you can coordinate your colors and find a rep at Bradley :)