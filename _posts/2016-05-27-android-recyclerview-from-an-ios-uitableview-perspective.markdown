---
layout: post
title:  "Android RecyclerView from an iOS UITableView perspective"
date:   2016-05-27 9:00:00
tags: Android
image: /img/mixer-screen-shot-1.png
description: "In iOS development, UITableView holds an important and prominent place at the table.  Apple has consistently used UITableView throughout iOS from the very first release of the iPhone and subsequently the SDK.  With and iOS perspective, let's take a look at the RecyclerView on Android and compare it to UITableView.<br/><br/>"
---

In iOS development, [UITableView][uitableview-link] holds an important and prominent place at the table.  Apple has consistently used UITableView throughout iOS since the very first release of the iPhone and subsequently the SDK.  The pattern is repeated throughout many of the apps on the platform as well.  If you develop in UIKit, you will undoubtedly have a strong understanding of [UITableView][uitableview-link], [UITableViewDataSource][uitableviewdatasource-link], and [UITableViewDelegate][uitableviewdelegate-link]. 

It's worth noting that Apple also introduced UICollectionView in iOS 6 which contains many of the powerful reuse paradigms of UITableView with even more flexibility in how items are displayed, allowing for things like grid views and other custom layouts.  In this article however, I'm going to use UITableView as an iOS reference since it is used more frequently and therefore probably more familiar to iOS developers.

### Given what we know about UITableView - what is available on Android for displaying lists? ###

When taking a look at Android, the ListView class seems to have similar functionality and also similar popularity in examples, references, and code out on the web.  However, with the release of Lollipop (Android 5.0) and version 7 of the Android Support Library, Google introduced a new collection display class called [RecyclerView][recyclerview-link]. 

Since RecyclerView is a part of the v7 support library, RecyclerView can be used with Android 2.1 (API level 7) and higher - good news if you need to support older versions of Android before Lollipop.

RecyclerView is a subclass of ViewGroup.  It displays a set of child view objects and handles the reuse of these child views similar to how the UITableView class in iOS handled UITableViewCell reuse.

### Cell and View Reuse ###

Why is view reuse important for mobile?  When scrolling through long lists of items on Android or iOS, views need to be generated for the on screen display of the items shown at any point in the list.  Depending on the complexity of the views, this can be very resource intensive and begin to block the main thread if not implemented with efficiency in consideration.  This leads to scroll lag and other issues that can lead to poor user experiences.

### UITableView Approach ###

 In the iOS SDK, UITableView was built with the concept of cell reuse from the beginning.  The [UITableViewDataSource][uitableviewdatasource-link] provides mechanisms for a controller to provide the UITableView with elements like the number of cells, the number of cell sections, and a view to be used for a specified cell.  The interface provides a mechanism to dequeue reusable cells from the UITableView itself so that common table elements can be represented using a relatively small number of reusable cells that are updated just prior to the cell view being presented on screen.  When a view is scrolled off screen, UITableView reuses it by providing it to the delegate and allowing the cell data and display to be updated.

Here's a glimpse of a simple implementation of the important delegate methods of the UITableViewDataSource:

<script src="https://gist.github.com/welbesw/993708871447e73f91063d1f0e63458a.js"></script>

## Android RecyclerView ##

In a similar manner to the UITableView class, [RecyclerView][recyclerview-link] on Android reuses the item views that are provided by an adapter.  The RecylerView's responsibility is simple: to recycle the item views used and position them on screen.  A LinearLayout can be used to provide a table like layout that positions items in a fashion similar to UITableView.  The RecyclerView works with two other classes to create and maintain the items it displays: an Adapter subclass and a ViewHolder subclass. 

### ViewHolder ###

The ViewHolder has a simple job - to hold on to a View.  Excellent class name :) 

### Layout Manager ###

When setting up the RecyclerView, you provide a layout manager that will tell it how to layout the views it maintains.  A simple table layout that presents items vertically can be accomplished with the LinearLayoutManager class.  There is also a GridLayoutManager or you can implement your own custom LayoutManager subclass to do something custom.

In order to add the RecyclerView to your Android Studio project, you will need to import the recylclerview-v7 library dependency.

[recyclerview-link]: https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html
[uitableview-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/
[uitableviewdatasource-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/occ/intf/UITableViewDataSource
[uitableviewdelegate-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/occ/intf/UITableViewDelegate