---
layout: post
title:  "iOS Wireless App Distribution for an Ad-Hoc Distribution Build"
date:   2011-01-18 11:00:00
categories: iOS
---

Since the release of iOS 4, first on the iPhone 4 and now on the iPad with 4.2, you can distribute your app wirelessly - allowing users to install the app directly on their device without using iTunes or the iPhone configuration utility. This makes it very easy to publish a build to a web location and allow users to browse to a website in safari on the device and simply click a link to install the app. For Ad-Hoc distributions, the install still requires the mobile provisioning profile to be installed on the device, but the install is very much simplified following this process. This also allows apps to be installed onto remote devices that are not located near the developer or without having to email the ipa file with detailed instructions. I'm a big fan of making the install process as simple as possible.

So here's the process...


* Follow Apple's documentation to create an Ad-Hoc distribution build with an appropriate provisioning profile.  Be sure to include the appropriate devices with the provisioning profile.
* In Xcode, select to build the app for Ad-Hoc distribution and build for a device. (You don't necessarily have to have a device connected, just choose the device option so that "Build and Archive" is available.)
* In the build menu select "Build and Archive".  This will provide a signed build that is ready for Ad-Hoc distribution and is archived.
* Open the "Organizer" (Window > Organizer) window to view the archived build of the app.  The archived builds appear in a list at the bottom of the Xcode organizer window.
* Once you click on the archived build, there should be a button at the top right that says "Share".  Click "Share" to bring up the app sharing options.  Select the appropriate provisioning profile and then click "Distribute for Enterprise". 
* In the "Distribution" window, enter the application url - the full url where you plan to publish the ipa file.  For example:  http://www.myuiviews.com/example/app_file_name.ipa  Also enter a title for the app (to be used while the app is installed) and click "Ok".  When prompted, pick a location on your system to save the IPA file.  This will also generate a plist file that will be used for the wireless install, based on the parameters that you provided.
* Upload the ipa and plist files to the web publish location.  Along with the generated plist and ipa, you'll need to publish the provisioning profile in the same location.
* Create an index.htm file with some simple HTML that links to your app install.  You may also wish to create links directly to the IPA and provisioning profile file is someone wishes to download the items and install them via iTunes or iPhone configuration utility.  Below is an example of the links that can be placed in the HTML file.

```html
<a href="itms-services://?action=download-manifest&url=http://www.myuiviews.com/example/app_file_name.plist">Install App</a> 

<a href="http://www.myuiviews.com/example/app_file_name.ipa">Download App</a>
```

* Send the web publish link to users to install the app.  Now all the user has to do is load the link in their iOS device in safari and click "Install" to install and run the app.  Since the app is being downloaded directly outside of iTunes, the OS prompts that user to confirm that they actually want to install and run the app. 
* That's it!  Next time you have to publish an ad-hoc build of your app, just follow the steps again and update the IPA file with the new version.  Then tell users to download the new update.



If you have an Enterprise Distribution certificate, a very similar wireless install process can be followed to allow the install of apps signed with your Enterprise certificate.  This really makes distributing apps internally a walk in the park.