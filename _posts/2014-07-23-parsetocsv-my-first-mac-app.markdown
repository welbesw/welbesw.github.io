---
layout: post
title:  "ParseToCSV: My First Mac App"
date:   2014-07-23 20:00:00
tags: iOS
description: "Earlier this month, my first Mac app, ParseToCSV was launched in the Mac App Store."
---

Earlier this month, my first Mac app, [ParseToCSV][app-store-link] was launched in the [Mac App Store][app-store-link].  If you're thinking, "Hey this thing is called My UIViews, not My NSViews", you're right.  Since 2008, I've been focused on UIKit and iOS development.  On occasion, I've dabbled in Cocoa, and ParseToCSV is the first app that I've actually released to the store.  Having a strong grasp on UIKit seems like a good foundation for venturing into Cocoa and Mac development.  There certainly are differences in the design approaches and environments, but as I've become more experienced in UIKit, I've found it to be a natural progression.  I sometimes wonder what it would have been to take the opposite path, having first started in Cocoa and then jumping into Cocoa Touch.

<a href="https://itunes.apple.com/us/app/parsetocsv/id889134301?mt=12"><img src="/img/parse-to-csv-screenshot.png" class="img-responsive center-block" alt="ParseToCSV"></a>

So why develop a Mac app?  It's actually a tool that I put together to extract data from Parse that serves as the back end for an iOS App that I developed for a client.  I've used Parse for several different apps and have come to appreciate the simplicity of getting an app launched without much back end effort.  However, one of the missing pieces that I found with Parse was an easy way to share the data in a simple spreadsheet format.  Parse allows exporting data in a JSON format that works well for the type of data that can be represented.  However, I've had several occasions where I just needed a simple spreadsheet version of the data table.  Enter [ParseToCSV][app-store-link].  

[ParseToCSV][app-store-link] is a simple utility that converts JSON export files from Parse.com into CSV files that can be loaded into spread sheet software. It was designed to handle large data sets and makes use of standard Mac app patterns.

Are you using [Parse](http://www.parse.com)? So take a look and let me know what you think.

<a href="https://itunes.apple.com/us/app/parsetocsv/id889134301?mt=12"><img src="/img/Download_on_the_Mac_App_Store_Badge_US-UK_165x40_0801.svg" class="img-responsive center-block" alt="Download on the Mac App Store"></a>

[app-store-link]: https://itunes.apple.com/us/app/parsetocsv/id889134301?mt=12