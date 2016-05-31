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

In a similar manner to the UITableView class, [RecyclerView][recyclerview-link] on Android reuses the item views that are provided by an adapter.  In order to add the RecyclerView to your Android Studio project, you will need to import the recylclerview-v7 support library dependency.  In Android Studio, navigate to *File -> Project Structure* and select the app module on the left.  Select the *Dependencies* tab.  Use the + button and choose *Library* to add a dependency.  Find the recyclerview-v7 library in the list and click OK.

<img src="/img/screen-shot-recyclerview-v7-add.png" class="img-responsive center-block" alt="Add recyclerview-v7 dependency">

### RecyclerView ###

The RecylerView's responsibility is simple: to recycle the item views used and position them on screen.  A LinearLayout can be used to provide a table like layout that positions items in a fashion similar to UITableView.  The RecyclerView works with two other classes to create and maintain the items it displays: an [Adapter][recyclerview-adapter-link] subclass and a [ViewHolder][viewholder-link] subclass. Let's walk through each of the classes needed and then tie them together in an Activity at the end.

### ViewHolder ###

The [ViewHolder][viewholder-link] has a simple job - to hold on to a View.  Excellent class name :)  

The ViewHolder maintains references to the views contained within the item view.  The ViewHolder provides some efficiency to the recycler by maintaining these references so that calls to **findViewById()** are called once when the item is created as opposed to every time the item is bound to a new item in the list.  The **findViewById()** call can be an expensive call depending on the contents of the view since it is essentially walking the view hierarchy looking for the identifier specified.  Minimizing the number of calls to **findViewById()** or other view setup calls really does offer a performance improvement.  

Here's a simple example of a ViewHolder that contains just one TextView.

<script src="https://gist.github.com/welbesw/09a4c9bb67811f40803a044cbb376b78.js"></script>

### Adapter ###

The [RecyclerView Adapter][recyclerview-adapter-link] follows the Adapter pattern used frequently in Android development.  The RecyclerView will communicate with the adapter whenever a new ViewHolder needs to be created for an item.

The **onCreateViewHolder()** method of the RecyclerView Adapter is called when a new ViewHolder is needed.  The method will inflate a view from a layout resource, or instantiate a view and return it to the recycler.  This method is only called when a new view holder is needed.  The RecyclerView should only need enough items to display on screen and provide some buffer when scrolling quickly.

The **onBindViewHolder()** method is called each time the RecyclerView Adapter uses a ViewHolder for an item that is to be displayed on screen.  This method will be called lots of times for the items in your list.  It is prudent to be efficient in this method and simply bind the data elements of the item to the ViewHolder Views.

The **getItemCount()** method is called by the RecyclerView to simply determine how many items are in the list.  The adapter is provided a reference to the items list when instantiated and will use that reference to determine the number of items in the list.

Below is a simple implementation of the [RecyclerView.Adapter][recyclerview-adapter-link] subclass that implements the methods described above:

<script src="https://gist.github.com/welbesw/12ec600d78bf54ee73a844189dada973.js"></script>

### Layout Manager ###

When setting up the RecyclerView, we provide a layout manager that will tell it how to layout the views it maintains.  A simple table layout that presents items vertically can be accomplished with the LinearLayoutManager class.  There is also a GridLayoutManager or you can implement your own custom LayoutManager subclass to do something custom.  For our simple example, we will use the LinearLayoutManager to build a simple vertical scrolling table.

### Tying it all together ###

Now that we have a ViewHolder and an Adapter, we will implement the RecyclerView in a [Fragment][fragment-link] subclass that will be instantiated in an [Activity][activity-link] and displayed on screen.  Implementing views inside a Fragment allows for some flexibility and maintainability in how user interfaces are built for Android, rather than simply implementing everything inside an Activity subclass.

Below is the RecyclerFragment implementation.  The fragment contains the list of items that will be displayed - in this case a simple array of strings.  The class also contains a reference to the RecyclerView that is a part of the fragment layout XML file and an instance of the ItemAdapter class.

The bulk of the setup work is performed inside the **onCreateView()** method.  After inflating the fragment's layout from the resource file, a reference to the RecyclerView is created.  A call to **setLayoutManager()** on the RecyclerView using the LinearLayoutManager sets up how the RecyclerView will handle layout.  Lastly, the **loadDataAndUpdateUI()** method connects the adapter to the recycler view with the list of items to be displayed.  It also has a simple loop that first loads data items.  This load of data items would most likely be an asynchronous load of data items from network or load of items from a database or preferences.  Once all of the components are connected, the RecyclerView has enough logic to dynamically load and display items from the list, while recycling items along the way.  

<script src="https://gist.github.com/welbesw/6ec55701719264ebce334c5290d230d2.js"></script>

<script src="https://gist.github.com/welbesw/bbc35b860aad0f83f4c08fda72157334.js"></script>

The [RecyclerView][recyclerview-link] provides a great pattern for implementing scrolling lists of items on Android.  The approach is very similar to how UITableView and UICollectionView on iOS operate.  If you have experience with the UITableView and UITableViewDataSource design patterns on iOS, you should be pretty comfortable using RecyclerView on Android.  Happy coding :)

[recyclerview-link]: https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html
[uitableview-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/
[uitableviewdatasource-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/occ/intf/UITableViewDataSource
[uitableviewdelegate-link]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/occ/intf/UITableViewDelegate
[viewholder-link]: https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html
[recyclerview-adapter-link]: https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html
[fragment-link]: https://developer.android.com/reference/android/app/Fragment.html
[activity-link]: https://developer.android.com/reference/android/app/Activity.html