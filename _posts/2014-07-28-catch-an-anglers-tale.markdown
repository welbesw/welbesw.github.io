---
layout: post
title:  "Catch: An Angler's Tale"
date:   2014-07-28 12:00:00
tags: iOS
description: "Inspired by my annual fishing trip in Wisconsin, I launched an app called Catch for iOS that enables anglers to keep track of the fish they catch."
---

I wouldn't say I'm an expert angler.  But I'm honored to be invited on an exclusive fishing trip every year with good friends from college.  It's a tradition that started shortly after college and is something that I look forward to each year.  However, in the past 3 years, the fishing trip always seemed to be scheduled during Apple's Developer Conference  week in June.  There are not a lot of things that would keep me from going on that fishing trip, but WWDC has come in the way.  I suppose the birth of Lily (number 3), in June of last year, also kept me away.  I guess that's a bit more important than a developer conference.

This year in June, I went.  Eric, James and I made our way North and went bass fishing on a lake on an island in a lake.  Yup - it's a secret spot.  Why is it secret?  Because the bass fishing is legend.  It's unlike any other fishing I've done.  Ever.  It's a common occurrence to have two or three fish in the boat at the same time.  Sometimes you can literally watch a Largemouth going after your bait and then take in the entire fight eventually landing in the boat.  It's good fishing.  Given the number of fish being pulled in, we had to have an [app to keep track][catch-app-link].  In years past, I've brought along a point and shoot camera and a hand held GPS unit to track some of the spots were we were catching fish.  Did I mention it's a bunch of engineers on the trip each year?  Of course we're tracking data.  It's also a bit competitive.  Not much has changed about the fishing trip since we started years ago, but technology sure has.  I remember thinking some company should put my phone, camera, and GPS unit together in one.  Some day I thought.

<div class="row">
	<div class="col-md-6">
		<img src="/img/will-catch.jpg" class="img-responsive center-block" width="300" alt="Will with a Largemouth Bass" style="padding: 10px 0px 10px 0px">
	</div>
	<div class="col-md-6">
		<img src="/img/eric-james-catch.jpg" class="img-responsive center-block" width="300" alt="Will with a Largemouth Bass" style="padding: 10px 0px 10px 0px">
	</div>
</div>

Having been challenged by Eric and James to put together an app only a few days before our trip, I had to come up with something.  I threw together a first version that allowed tracking fish caught with location, photo, species, and length.  I then added a simple map and plotted the catches.  It was not optimized and had some rough edges and hard coded areas, but it was enough to record some data for the weekend.  Off we went.

As we travelled along I-43, I demoed the functionality, plotting my first catch on the interstate.  I hadn't yet added the ability to delete any data, so that catch was with us for the rest of the trip.  When we finally arrived at our destination and got out on the water, the app proved it was up to the challenge.  I was able to keep track of catches as we hauled them in.  I'd record a photo, length, etc.  However, my lack of polish left several pain points that needed to be addressed.  As Eric and James were pulling in fish, I was wasting time tracking the data. Recording a new catch was taking longer with each fish caught. My real world use proved valuable in assessing features and optimizations for a first version.

After the trip, I developed a list of improvements that needed to be completed before going to the App Store.  I checked things off the list to get to an MVP (minimally viable product).  Yesterday, [Catch][catch-app-link] launched in the iOS App Store.  If you fish and want to keep track of your catches, [check it out][catch-app-link].  I hope to have several more trips yet this year to put [Catch][catch-app-link] through more real world testing.  This is work, right?

<div class="row">
	<div class="col-md-12">
		<a href="https://itunes.apple.com/us/app/catch/id899392064?mt=8"><img src="/img/catch-app-icon.png" class="center-block" alt="Catch App"></a>
		<br/>
		<a href="https://itunes.apple.com/us/app/catch/id899392064?mt=8"><img src="/img/Download_on_the_App_Store_Badge_US-UK_135x40.svg" alt="Download Catch on the App Store" class="center-block" /></a>
		<br/>
	</div>
</div>

[catch-app-link]: https://itunes.apple.com/us/app/catch/id899392064?mt=8