---
layout: post
title:  "Handling Portrait And Landscape in iOS 7 at App Lanuch"
date:   2014-03-27 16:00:00
tags: iOS
description: "I've recently been updating one of my iPad apps with a new iOS 7 version that supports both landscape and portrait orientations. Using storyboards, I laid out the interface in portrait. I then setup a custom method to setup the views for portrait or landscape depending on the orientation of the device. Or so I thought."
---

I've recently been updating one of my iPad apps with a new iOS 7 version that supports both landscape and portrait orientations.  Using storyboards, I laid out the interface in portrait.  I then setup a custom method to setup the views for portrait or landscape depending on the orientation of the device.  Or so I thought.  I struggled to get the app to properly setup the interface orientation after launching in landscape.  Everything worked great when the app launched in portrait, but the rotation method was not getting called at app launch, so views were not properly setup when launched in landscape.  Here's how I solved it.

### Setup the app to handle portrait and landscape. ###

In the project settings area of the app project, select the appropriate orientation settings.

<img src="/img/orientation-options.png" class="img-responsive center-block" alt="Orientation Options">

### Layout StoryBoard in portrait ###

When laying out your view controllers, use portrait orientation.  Apple's [UIViewController](https://developer.apple.com/library/ios/documentation/uikit/reference/UIViewController_Class/Reference/Reference.html) documentation recommends this for controllers that will support multiple orientations.

### Handle view rotations with animation when the device is rotated ###

To support view rotations when the device is rotated, implement the willAnimateRotationToInterfaceOrientation method on your view controller and setup the views for the appropriate orientation.  I've implemented a custom method to setup the views for a given rotation.  AutoLayout could be used, but I've accomplished the layout with a custom method for this specific case.  The willAnimateRotationToInterfaceOrientation is called inside of the view controllers animation block, so you can set the appropriate animatable properties on your view like the frame, etc.  Here's the source:

```objective_c
- (void)willAnimateRotationToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
{
    [super willAnimateRotationToInterfaceOrientation:toInterfaceOrientation 
    										duration:duration];
    
    //Setup the views for a given orientation - sets the frames and images appropriately
    [self setupViewsForOrientation:toInterfaceOrientation];
}
```

### Handle landscape on first app launch ###

So the above will animate the views to the appropriate locations when the orientation changes.  However, the willAnimateRotationToInterfaceOrientation only gets called for view controllers that are currently displayed on the screen when the device rotates.  It will not get called when the app is first launched.  THis means that if the app is launched in landscape, that views are not properly set up, they will still be set for a portrait layout.  To change this, implement the viewDidLayoutSubviews method on your view controller to change the layout.  This method gets called at the appropriate time in the view life-cycle to setup the subviews before being displayed after app launch.  Here's the code:

```objective_c
-(void)viewDidLayoutSubviews
{
    [super viewDidLayoutSubviews];
    
    if(self.isFirstLaunch) {
        UIInterfaceOrientation orientation = [UIApplication sharedApplication].statusBarOrientation;
        [self setupViewsForOrientation:orientation];
        self.isFirstLaunch = NO;
    }
}
```

You'll notice that I've defined a BOOL flag called isFirstLaunch that is initially set to YES when the app launches.  This provides the mechanism to check if the call to viewDidLayoutSubviews is the first run through or not.  That prevents calling this method every time viewDidLayoutSubviews is called.  We already have the view rotation case handled in the willAnimateRotationToInterfaceOrientation method.