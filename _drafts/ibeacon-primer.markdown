---
layout: post
title: "iBeacon Primer"
date: 2014-01-01 9:41
tags: iOS iBeacon
---

With the launch of iOS 7, Apple incorporated support for a new Bluetooth Low Energy (LE) protocol they defined called iBeacon.  With all of the other changes in iOS 7, iBeacon fell a bit below the radar for the general tech press. Over the past several weeks, it seems that the tech press has picked up on the feature and iBeacon has been getting some more attention.  So what is iBeacon all about? 

##iBeacon - A new indoor positioning system

iBeacon is a new indoor positioning system that utilizes Bluetooth low energy transmitters that can notify nearby iOS 7 devices of their presence.  It's incorporated into iOS to provide simple notification and ranging information about beacons near by.  A beacon broadcasts a globally unique identifing number (UUID) as well as application specific major and minor numbers (integers).  iOS apps can register with the operating system to be alerted when a beacon of a specific UUID is discovered.  Once discovered, an app can perform a simple task in the background like posting a local notification for the user on the device.  There are a wide range of applications for the system that equip apps with some unique location context.

##How could iBeacon be used?
Much of the press coverage and excitement about iBeacon has been related to retail implementations.  Some retailers have already started implementing iBeacons into their stores, incorporating the sytem into their store app.  As a user walks over to an area of the store with an iBeacon, they could be presented with context specific information with additional details about products or product offers.  As more consumers are augmenting their shopping experience with their mobile devices, it's easy to see how presenting this contextual information right on their device could benefit both the retailer and consumer.  However, the retail experience is less exciting to me than a lot of other possibilities.  

One of the first examples that Apple talked about at WWDC this past year was a digital experienc within an art musuem, namely the Louvre.  As a  

##Getting into the details

When I was at WWDC in July, I attended the session on iBeacon presented by the Apple App Store App engineers.  They gave some great example of what could be done with iBeacon and how it was being incorporated into Apple's retail experience.  On the plane ride home from WWDC, iBeacon had peaked my interest enought to investigate it in the beta SDK and write a simple app to test it out.  iOS devices can be both transmitters and recieves for the iBeacon protocol, so that made having a broadcast beacon test device pretty simple.