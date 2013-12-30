---
layout: post
title:  "NSDate Comparison - Which date is older?"
date:   2011-01-10 10:00:00
categories: iOS Objective-C
---

I had to lookup how to do date comparisons in Objective-C this morning to be able to determine if one date is older than another:

```obj-c
NSDate * dateOne = [NSDate date];
NSDate * dateTwo = [NSDate date];

if([dateOne compare:dateTwo] == NSOrderedAscending) {
    // dateOne is before dateTwo
}
```

The above check will determine if dateOne is before dateTwo. The comparison will return one of the following codes which could potentially be used in a switch case if needed:

```obj-c
NSOrderedAscending //dateOne before dateTwo
NSOrderedSame  //dateOne and dateTwo are the same
NSOrderedDescending  //dateOne is after dateTwo
```