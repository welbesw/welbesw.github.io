---
layout: post
title:  "Looking for a way to print shipping labels on your Mac?"
date:   2016-01-04 15:34:00
tags: Swift
image: /img/easyshipper-screenshot1.png
description: "I recently created and launched Easy Shipper on the Mac app store, enabling Mac users to quote, buy and print shipping labels."
excerpt: "I recently created and launched Easy Shipper on the Mac app store, enabling Mac users to quote, buy and print shipping labels..<br/><br/>"
---

Look no further :)  Check out [Easy Shipper on the Mac App Store][easy-shipper-link].

<a href="https://itunes.apple.com/us/app/easy-shipper-ship-usps-ups/id1055793428?mt=12">
	<img src="/img/easyshipper-screenshot1.png" alt="Easy Shipper for Mac" class="img-responsive" />
</a>

This past fall, I discovered [EasyPost][easypost-link] when [Alex Hardin](https://twitter.com/alexchardin) posted a link to an app called [Shipper](http://www.shipperapp.net) that he had developed for Windows users.  Having shipped thousands of packages over the past 10 years, I've often found myself looking for a better option to quote, buy and print shipping labels.  As a developer, on occasion, I would start investigating the APIs available from shipment companies like the US Postal Service, UPS, and FedEx, but found that integrating with them would not be easy.  (I know, a government agency and big corporations not making things easy? Weird.)  Most also required that you get approved as a shipping vendor with them - ruling out the small guys.  

In comes [EasyPost][easypost-link] to the rescue!  Launched in 2012, they built a [really nice API](https://www.easypost.com/docs/api) for developers to program against to buy and print shipping labels across many differnt carriers.

Armed with [EasyPost's API](https://www.easypost.com/docs/api), I set about creating a Mac app to use for my every day shipment processing to replace what I was currently using, Endicia's Mac app.  Endicia charges a monthly fee of $14.95 that I didn't want to pay if there was a way to avoid it.  EasyPost charges a fixed per label fee of $0.05.  Depending on monthly shipment volume, that could be higher than $14.95, but one caveat was that EasyPost offers US Postal Service Commercial Plus pricing which is cheaper than what Endicia offers.  So for my case, even with a higher volume, EasyPost would be more economical.  It also allowed me to print UPS and FedEx labels right in the same app as well.

This was a fun project.  After finishing a version for myself, I refined it for the App Store and launched.  I integrated printing the labels with the Zebra 2844 thermal label printer.  It was pretty exciting when my app connected to the thermal printer for the first time and out popped a shipping label!  :)  Fun stuff for those of us that mostly move bits around.... to actually generate tangible output that popped out of a printer.  Cool.

If you print labels from your Mac, [check it out on the Mac App Store][easy-shipper-link] and let me know what you think :)

[Easy Shipper][easy-shipper-link] enables Mac users to quote, buy and print shipping labels.  It uses the [EasyPost API](https://www.easypost.com/docs/api) and requires an [EasyPost][easypost-link] account.

<a href="https://itunes.apple.com/us/app/easy-shipper-ship-usps-ups/id1055793428?mt=12">
	<img src="/img/easyshipper-screenshot2.png" alt="Easy Shipper for Mac" class="img-responsive" />
</a>

[easy-shipper-link]: https://itunes.apple.com/us/app/easy-shipper-ship-usps-ups/id1055793428?mt=12
[easypost-link]: http://www.easypost.com