---
layout: post
title:  "iOS Network Link Conditioner"
date:   2014-01-15 17:00:00
tags: iOS
description: "Need to test your app on slow cellular connections? Bring on the iOS network link conditioner! As a best practice for mobile development, developers and testers should test network connected apps under lots of different network conditions on real mobile devices."
---

Need to test your app on slow cellular connections?  Bring on the iOS network link conditioner!  As a best practice for mobile development, developers and testers should test network connected apps under lots of different network conditions on real mobile devices.  The simulator can be great for some development activity, but a majority of your development and testing should be on a real device.

Mobile devices connect to the Internet via a wide range of connection types and speeds.  Network unreliability is an expected condition on mobile.  Our mobile devices push the paradigms of network connectivity into new space.  So how do we test for these different conditions?  Thankfully, Apple has included a network link conditioner right in iOS that helps to simulate different types of network connections right on a development device.

### How to access and use the iOS link conditioner ###

1. Ensure your device is enabled as a development device via Xcode.  If you haven't already done so, inside Xcode, open up the Organizer window.  Tap on the Devices tab.  Select your connected device and view the information in the right panel.  Click the "Use for Development" button if you haven't already done so.  Now your device is enabled for development and the settings menu will include a new menu option.

2. Open settings in the iOS 7 device.  Scroll down to find the "Developer" menu option.

 <img src="/img/link-conditioner-4.png" class="img-responsive center-block" alt="iOS 7 Settings: Developer Menu Option">

3. Tap "Developer" and you'll see the developer options below listed.  

 <img src="/img/link-conditioner-1.png" class="img-responsive center-block" alt="iOS 7 Settings: Developer">

4. Tap on "Network Link Conditioner" to view the options.

 <img src="/img/link-conditioner-3.png" class="img-responsive center-block" alt="iOS 7 Settings: Link Conditioner">

5. Choose one of the pre defined profile options like 3G or Edge to simulate a slower network connection.  You can also add a custom profile where you define items like the bandwidth, packet loss, delay, etc.

 <img src="/img/link-conditioner-2.png" class="img-responsive center-block" alt="iOS 7 Settings: Add Link Conditioner Profile">

6. Tap the "Enable" switch to turn on the network link conditioner with the selected profile.

7. Test your app!  With the link conditioner turned on, go test your app and see how it operates under different network conditions.  Think about the areas of the app that make network requests and ensure that your app responds appropriately to time outs, data loss, etc.

8. Don't forget, when you're done testing, turn the network conditioner off!  It'll be pretty frustrating trying to do most things on your device with it turned on.
