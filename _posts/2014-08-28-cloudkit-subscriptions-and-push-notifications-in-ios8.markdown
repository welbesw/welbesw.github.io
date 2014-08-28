---
layout: post
title:  "CloudKit Subscriptions and Push Notifications in iOS8"
date:   2014-08-28 15:00:00
tags: iOS
description: "Let's take a look at how to use push notifications with CloudKit subscriptions in iOS 8."
---

<img src="/img/cloudkit.png" alt="Apple's CloudKit for iOS 8" style="float:left; padding-right:20px; padding-bottom:20px;" />

As I have been implementing an app using Apple's new [CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/iCloudDesignGuide/DesigningforCloudKit/DesigningforCloudKit.html) for iOS 8, I've come across a few hurdles, as expected.  It's a brand new platform as a service offering from Apple, a company that doesn't yet have a strong history in the domain.  That being said, it is a pretty compelling offering and there is a lot to like.

CloudKit in iOS 8 supports sending push notifications to clients based on CloudKit Subscriptions.  Subscriptions are queries that your app registers with CloudKit to be notified any time something related to the query changes in the cloud.  Almost like long polling without polling.  A common use case for this is to setup a query on a data type to be notified any time a new record is added.  How does the client find out about the new data?  A push notification.  And for these push notifications, there's no setup of a push service, certificates, etc.  It just works.  Once you've selected to receive background push notifications, you app is ready for CloudKit to send tickler updates.

However, in registering for a subscription, I noticed that the "alert" message for the push notification would be specified at the time of creation of the subscription.  For testing, I simply set the alert to "New Record!".  But wait.  How would I set that notification alert text to something appropriate for the actual notification?  I can just image a user getting a notification like "New Record!".  No context.  No information about what that new record is.  Probably a candidate for App Store rejection based on the user experience.

So how can we set the text of a CloudKit push notification just before it's sent to the app?  We can't.  Oh no.  All hope lost.  Time for a hike with the family.

After an hour of hiking in the Wisconsin woods with Lily on my back, Lincoln running around, and Eliana holding my hand fearful of the bugs in the forest, it hit me.  Let the push notifications come in the background without presenting to the user and use the app processing to figure out what the notification is for and then create a UILocalNotification to be presented with appropriate context.  The approach is similar to handing iBeacon notifications in the background.  Wake the app in the background, process and then send a local notification.  The notification payload for a subscription push notification also contains the record id of the changed record so you can retrieve it from CloudKit.  And, you can also set certain fields of the record to be sent directly in the push notification so that you can process even without having to get any more data from CloudKit.  This makes it possible to set the push message to something like "New message from Johnny Appleseed: Hey buddy...".

As I got back to prototyping the approach, I took another look at the documentation on subscription push notifications, and it appears that this is the approach that Apple is recommending.  They don't make it very explicit and don't provide an example in their [CloudKitAtlas sample code](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html) either.

Below is an example of processing remote notifications and setting a UILocalNotification in the App Delegate:

```obj-c
-(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
{
    NSLog(@"Push received.");
    
    NSDictionary * apsDict = [userInfo objectForKey:@"aps"];
    
    NSString * alertText = [apsDict objectForKey:@"alert"];

    //TODO: get the record information from the notification and create the appropriate message string.
    
    if(application.applicationState == UIApplicationStateActive) {
        UIAlertView * alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:alertText delegate:nil cancelButtonTitle:@"Ok" otherButtonTitles: nil];
        [alert show];
    } else {
    
        if([application currentUserNotificationSettings].types & UIUserNotificationTypeAlert) {
            UILocalNotification * localNotification = [[UILocalNotification alloc] init];
            localNotification.alertBody = alertText;
            [application presentLocalNotificationNow:localNotification];
        }
    }
    
    completionHandler(UIBackgroundFetchResultNoData);
}
```