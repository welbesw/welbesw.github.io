---
layout: post
title:  "CoreAudioMixer - An investigation into CoreAudio (AUGraph and AVAudioEngine) on iOS"
date:   2016-02-26 15:00:00
tags: Objective-C
image: /img/mixer-screen-shot-1.png
description: "In an effort to learn about CoreAudio on iOS, I implemented CoreAudioMixer for iOS with the goal of simply mixing a few audio tracks together via the CoreAudio APIs."
excerpt: "In an effort to learn about CoreAudio on iOS, I implemented CoreAudioMixer for iOS with the goal of simply mixing a few audio tracks together via the CoreAudio APIs.<br/><br/>"
---

In an effort to dive in and learn about CoreAudio on iOS, I recently implemented an iOS app call [CoreAudioMixer][github-link].  The app simply provides a way to mix a few audio tracks together via the CoreAudio APIs.  The focus of the implementation was CoreAudio, not the user interface, so I kept the interface implementation to a minimum: a play button and a couple sliders :) 

You can download the source code on [GitHub][github-link].

<a href="https://github.com/welbesw/CoreAudioMixer">
	<img src="/img/mixer-screen-shot-1.png" alt="CoreAudioMixer" class="img-responsive pull-left" />
</a>

The app uses two different audio files, one with drums and one with guitar, and allows the user to mix them together.

There are two different implementations of interfacing with CoreAudio. First with AudioToolbox using [AUGraph](https://developer.apple.com/library/prerelease/ios/documentation/AudioToolbox/Reference/AUGraphServicesReference/index.html#//apple_ref/c/func/AUGraphAddNode) and second with [AVAudioEngine][ave-link], Apple's newer audio API released with iOS 8. The segmented control at the top allows the user to select which implementation to interact with.

Once an implementation is selected, the audio files are loaded into the buffer and prepared for playing.  Press "Play" and then adjust the volume on the individual tracks via the sliders. Playback can be stopped and started via the "Play/Stop" button. The output volume level is shown as a percentage between 0% and 100%. The guitar is output to the left channel and the drums are output to the right channel.

In 2014, with the release of the CoreAudioEngine API, Apple hosted a WWDC session called ["AVAudioEngine in Practice"](http://devstreaming.apple.com/videos/wwdc/2014/502xxvo7vov799k/502/502_avaudioengine_in_practice.pdf) that provided a nice overview of the new API and how it could be implemented.  I referened this session while developing the CoreAudioEngine implementation for the app.

For more about CoreAudio, visit the iOS [CoreAudio Apple Developer Library](https://developer.apple.com/library/ios/documentation/MusicAudio/Conceptual/CoreAudioOverview/CoreAudioEssentials/CoreAudioEssentials.html).

[github-link]: https://github.com/welbesw/CoreAudioMixer
[ave-link]: https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVAudioEngine_Class/index.html