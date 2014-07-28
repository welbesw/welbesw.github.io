---
layout: post
title:  "Hello Swift Playground with UIKit in Xcode 6"
date:   2014-07-25 13:00:00
tags: iOS
description: "With the launch of Xcode 6 and iOS 8 at WWDC this year, Apple also introduced a new programming language called Swift.  One of the cool new features of Swift that's integrated into Xcode 6 is Playgrounds - a place to play with code and view output in real time."
---

With the launch of Xcode 6 and iOS 8 at WWDC this year, Apple also introduced a new programming language called Swift.  One of the cool new features of Swift that's integrated into Xcode 6 is Playgrounds - a place to play with code and view output in real time.  Over the years developing on iOS, I've often cluttered my projects directory with projects named SomethingTest, HelloSomething, etc. as I create small projects to test some code.  Enter playgrounds in swift to replace those types of projects that never got committed.  It seems that playgrounds could now follow inside a project and enable better collaboration with other team members on development code.

So let's take a step by step look at getting started with a Swift playground.

If you haven't already, download and install Xcode 6 beta from [developer.apple.com](http://developer.apple.com).  Open Xcode 6 and you'll see a new option on the main menu titled "Get started with a playground".  Click that.

<img src="/img/xcode-playground-option.png" class="img-responsive center-block" alt="Xcode 6 Playground Option" style="padding: 10px 0px 10px 0px">

Next, type in a name like HelloSwift for your new swift file.  We'll be using an iOS playground, but you could 
also choose OS X.  When prompted, choose a location for the file.  It's nice that this could now be within another project directory structure just like other project files.

<img src="/img/xcode-new-swift-project.png" class="img-responsive center-block" alt="Xcode 6 Playground Option" style="padding: 10px 0px 10px 0px">

Once you've selected your location, Xcode loads your new playground.  On the left is the code window and on the right is the output area.  You'll see the output built on the right as you progress thru code.  Pretty cool.  UIKit is already included so that you can start working with elements of the UIKit framework right away.  You'll see a simple string variable defined in Swift as well.

<img src="/img/xcode-playground-start.png" class="img-responsive center-block" alt="Xcode 6 Playground Start" style="padding: 10px 0px 10px 0px">

Let's start by creating and viewing a UILabel.  A simple example of how you might apply changes to the properties of a label in real time and view them without having to compile and run your app.  This flow lets you dial in your changes while playing with the code.

Add the following code to create a new UILabel:

```
import UIKit

var str = "Hello, playground"

//Define a UILabel with a frame and set some display text
let helloLabel = UILabel(frame: CGRectMake(0.0, 0.0, 200.0, 44.0))
helloLabel.text = str;

helloLabel
```

Now we can get a view of the label using the Assistant Editor.  Open the Assistant Editor by going to **View -> Assistant Editor -> Show Assistant Editor**.  You'll see a new view on the right with the Console Output.  We haven't sent anything to the console, so there's nothing there.  Let's add our label view.  Hover over the "UILabel" description that appears to the right of the final line with "helloLabel".  Tap the little white circle to add the UILabel to the assistant editor as shown below.

<img src="/img/swift-playground-uilabel.png" class="img-responsive center-block" alt="Swift Playground with UILabel" style="padding: 10px 0px 10px 0px">

Now we can see the label with it's text that we set in the code above.  But that's kind of a boring label.  Let's add some color and round out the corners.  Type in the following code below the line that sets the text of the label, but above the final line with "helloLabel".  I encourage you to type it in and watch the output change in the Assistant Editor as you type.

```
//Change the color and round the corners of the label
helloLabel.backgroundColor = UIColor.greenColor()
helloLabel.layer.masksToBounds = true
helloLabel.layer.cornerRadius = 10.0
helloLabel.textAlignment = NSTextAlignment.Center
```

Your label should be a lot greener and not as sharp now.

<img src="/img/swift-playground-label-green.png" class="img-responsive center-block" alt="Swift Playground with UILabel now green with rounded corners" style="padding: 10px 0px 10px 0px">

Another neat aspect of playgrounds and the Assistant Editor is that you can inspect the output at different points in the program flow.  So for instance, you could add another view of the UILable to the Assistant Editor before you've applied the changes to background color and corner radius to see what it looks like before that.  Just click on the little white circle next to the "UILabel" to the right of the line where you'd like to inspect.

Now you have a nice new label that you can copy and paste into your next iOS project.  While we haven't done much here, I'm sure your can see the value that playgrounds can provide.  Go and play some more.

Will you be using playground with Swift in Xcode 6?

