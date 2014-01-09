---
layout: post
title: "iBeacon Primer"
date: 2014-01-09 17:20
tags: iOS iBeacon
---

With the launch of iOS 7, Apple incorporated support for a new Bluetooth Low Energy (LE) protocol they defined as iBeacon.  With all of the other visual changes in iOS 7, iBeacon fell a bit below the radar. Over the past several weeks, it seems that the tech press has picked up on the feature and iBeacon has been getting some more attention.  So what's it all about? 

###iBeacon - A new indoor positioning system

iBeacon is a new indoor positioning system that utilizes Bluetooth low energy transmitters that can notify nearby iOS 7 devices of their presence.  It can be incorporated into iOS apps to provide simple notification and ranging information about beacons near by.  A beacon broadcasts a unique identifier (UUID) as well as application specific major and minor numbers (integers).  iOS apps can register with the operating system to be alerted when a beacon of a specific UUID is discovered.  Once discovered, an app can perform a simple task in the background like posting a local notification for the user on the device.  Apps can also range beacons and determine the approximate distance away, from about 30 meters down to about 10 centimeters.  

There are a wide range of applications for the system that equip apps with some unique location context.

###So how could iBeacon be used?
Much of the press coverage and excitement about iBeacon has been related to retail implementations.  Some retailers have already [started implementing iBeacons into their stores](http://appleinsider.com/articles/13/11/20/macys-begins-pilot-test-of-apples-ibeacon-in-flagship-new-york-san-francisco-stores) and incorporating the system into their store app.  As a user walks over to an area of the store with an iBeacon, they could be presented with context specific information with additional details about products or product offers.  As more consumers are augmenting their shopping experience with their smart phones, it's easy to see how presenting this contextual information right on their device could benefit both the retailer and consumer.  However, the retail experience is less exciting to me than a lot of other possibilities.  

One of the first examples that Apple talked about at WWDC this past year was a digital experience within an art museum.  Let's assume that this example art museum has an app with information about the collections and works of art that they have.  How cool would it be to present specific information about important pieces of art when a user is standing right in front of them?  The museum could place low power, low cost, iBeacon devices at key pieces that broadcast a unique identifier.  If the art viewer has the app installed and they are near a beacon, when they pull their phone out of their pocket and turn on the screen, a notification from the app could appear about the item in range.  If the user acts on the notification, the user could be taken to a details screen of information about the piece and presented with even more information and context about the subject.  The real world indoor art experience is augmented with powerful digital contextual information.  (Psst... hey app developers, here's your chance to politely nudge your users at the OS level to come take a look inside your app.)

I expect that there are a lot of other applications where iBeacon could be used.  Lots of different large, indoor venues could augment their user experiences with location based app content.  Major League Baseball has already begun [experimenting with the technology](http://mashable.com/2013/09/26/mlb-at-the-ballpark-app/) at a few select ball parks, presenting users of their ["At the Ballpark"](https://itunes.apple.com/us/app/mlb.com-at-the-ballpark/id513135722?mt=8) app with location aware content.  How about large conferences and industry trade shows?  Enable booths to use iBeacon transmitters to give users context about the vendors at each booth.  How about a school or university that can enable students to view information specific to a classroom with iBeacon?  Or a corporation campus with information about each conference room or lab as you walk in?  There's lots of possibilities for iBeacon.  It's going to be interesting to see if it takes off with developers and starts popping up in more real world places.  

###Homing in on the details

Alright.  Lots of great ideas, but let's take a look at how iBeacon can be incorporated into an app.

When I was at WWDC 2013, I attended the session on iBeacon presented by the Apple Store App engineers.  They gave some great example of what could be done with iBeacon and how it was being incorporated into Apple's retail experience.  On the plane ride home from WWDC, iBeacon had peaked my interest enough to investigate it in the beta SDK and write a simple app to test it out.  iOS devices can be both broadcasters and monitors for the iBeacon protocol, so that made having a broadcast beacon test device pretty simple. (I'm not sure if the FAA would have approved of the beacons i got going on the flight :)

Since I wasn't able to post anything publicly during the iOS 7 beta period, the project I had put together got put on the back burner.  However, now that iOS 7 is out and I've had a chance to revisit it, I've posted a couple sample projects to [GitHub](https://github.com/welbesw/iBeaconExamples). These examples illustrate how to incorporate iBeacon into an app.  Let's take a look at some of the highlights.

####Beacon Monitor

The Beacon Monitor project will simply monitor for a specific iBeacon region type and show any beacons of that type in a simple table view.

The iBeacon functionality is incorporated into the CoreLocation framework within iOS.  The newly added CLBeaconRegion and CLBeacon classes are used to define the beacons that an app will be monitoring.  The CLBeaconRegion defines a type of region that is based on an iOS device's proximity to an iBeacon broadcasting via Bluetooth LE.  To start using iBeacon in your app, you can define a beacon region that you will be monitoring for and then have iOS notify your app when you enter or exit the beacon region.  For the purposes of region monitoring, I've created a simple [BeaconRegionManager](https://github.com/welbesw/iBeaconExamples/blob/master/BeaconMonitor/BeaconMonitor/BeaconRegionManager.m) singleton class.  In the init method, we instantiate the CLLocationManager.

```obj-c
self.locationManager = [[CLLocationManager alloc] init];
self.locationManager.delegate = self;
```

To start monitoring for iBeacons, we use the CLBeaconRegion class:

```obj-c
-(void)startBeaconMonitoring
{
    //Register the UUID beacon region to be monitoring
    //NOTE: The UUID defined here must match the UUID that
    //is being broadcast by your iBeacon broadcaster
    NSString * uuidString = @"BEE44022-97E5-4F0C-A100-0C43C114CCF5";
    NSUUID * uuid = [[NSUUID alloc] initWithUUIDString:uuidString];
    
    self.beaconRegion = [[CLBeaconRegion alloc] initWithProximityUUID:uuid
                                                           identifier:@"TestBeacon"];
    self.beaconRegion.notifyEntryStateOnDisplay = YES;
    
    //Tell iOS to monitor for the beacon specified and alert us
    //when one comes in range or goes out
    [self.locationManager startMonitoringForRegion:self.beaconRegion];
}
```

Once iOS detects that the device is within a beacon region, the CoreLocationManagerDelegate method didEnterRegion will get called:

```obj-c
- (void)locationManager:(CLLocationManager *)manager
         didEnterRegion:(CLRegion *)region
{
    NSLog(@"didEnterRegion: %@", region.identifier);
    
    //If we've entered a beacon region, start ranging the beacons
    if([region isKindOfClass:[CLBeaconRegion class]] &&
       [region.identifier isEqualToString:self.beaconRegion.identifier]) {
        CLBeaconRegion * beaconRegion = (CLBeaconRegion*)region;
        [self.locationManager startRangingBeaconsInRegion:beaconRegion];
    }
}
```

Once we know that the device is in the region, the call to startRangingBeaconsInRegion on CLLocationManager will cause the location manager to start ranging and subsequently cause calls to didRangeBeacons on the delegate.  We implement the delegate method to tell our consumers that the range information has changed each time the delegate method is called:

```obj-c
- (void)locationManager:(CLLocationManager *)manager
        didRangeBeacons:(NSArray *)beacons inRegion:(CLBeaconRegion *)region
{
    for(CLBeacon * beacon in beacons) {
        NSLog(@"didRangeBeacon: major:%d minor:%d in region: %@",
              [beacon.major intValue], [beacon.minor intValue], region.identifier);
        NSLog(@"didRangeBeacon proximity: %d", beacon.proximity);
        NSLog(@"didRangeBeacon accuracy: %f", beacon.accuracy);
    }
    
    self.beacons = beacons;
    if(self.delegate != nil) {
        [self.delegate beaconRegionManager:self didRangeBeacons:self.beacons];
    }
}
```

By using a protocol on the BeaconRegionManager class, our view controllers can implement the delegate and update the UI any time new beacons are discovered or the range values change.

Take a closer look at the [Beacon Monitor](https://github.com/welbesw/iBeaconExamples/tree/master/BeaconMonitor) project to see the full implementation.

####Beacon Broadcaster

So now that we have a beacon monitor, we need a beacon.  The Beacon Broadcaster app example simply uses another iOS device as the beacon broadcaster.  For a beacon broadcaster, we need to incorporate the CoreBluetooth library and setup a simple peripheral.  I've implemented a simple [BeaconBroadcastManager](https://github.com/welbesw/iBeaconExamples/blob/master/BeaconBroadcaster/BeaconBroadcaster/BeaconBroadcastManager.m) singleton class that instantiates a peripheral manager in the init method.  (For the following examples, assume self refers to the [BeaconBroadcastManager](https://github.com/welbesw/iBeaconExamples/blob/master/BeaconBroadcaster/BeaconBroadcaster/BeaconBroadcastManager.m))

```obj-c
self.peripheralManager = [[CBPeripheralManager alloc] initWithDelegate:self queue:nil];

NSString * uidString = @"BEE44022-97E5-4F0C-A100-0C43C114CCF5";
self.proximityUUID = [[NSUUID alloc] initWithUUIDString:uidString];
self.beaconRegionIdentifier = @"TestBeacon";
```
With the peripheral manager setup, there's now a public method on the manager class to start advertising (broadcasting) a region.

```obj-c
-(void)startAdvertisingWithMajor:(NSInteger)major minor:(NSInteger)minor
{
    if(self.peripheralManager.isAdvertising) {
        [self.peripheralManager stopAdvertising];
    }
    
    //Set the beacon region
    self.beaconRegion = [[CLBeaconRegion alloc] initWithProximityUUID:self.proximityUUID
                                                                major:major
                                                                minor:minor
                                                           identifier:self.beaconRegionIdentifier];
    
    NSDictionary * dict = [self.beaconRegion peripheralDataWithMeasuredPower:nil];
    [self.peripheralManager startAdvertising:dict];
}
```

Take a closer look at the [Beacon Broadcaster](https://github.com/welbesw/iBeaconExamples/tree/master/BeaconBroadcaster) project for the full implementation.

###Go play!
Enough of this chatter, go checkout the sample projects on [GitHub](https://github.com/welbesw/iBeaconExamples) and start playing around with iBeacon on your devices.  If you have an Apple Developer account, you can also take a look at the [AirLocate](https://developer.apple.com/downloads/index.action?name=WWDC%202013) example project that Apple provided along with the [WWDC 2013 session 310 video, "Harnessing iOS to Create Magic in Your Apps"](https://developer.apple.com/wwdc/videos/).