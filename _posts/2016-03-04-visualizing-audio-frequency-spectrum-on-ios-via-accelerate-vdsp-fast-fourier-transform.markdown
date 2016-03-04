---
layout: post
title:  "Visualizing the audio frequency spectrum on iOS via the Accelerate Framework and FFT(Fast Fourier Transform)"
date:   2016-03-04 15:00:00
tags: FFT
image: /img/mixer-screen-shot-1.png
description: "It's not every day that I get to use a Fourier Transform, but today was one of those days.<br/><br/>"
---

It's not every day that I get to use a Fourier Transform, but today was one of those days.  Over the past week, I've had a chance to experiment with the [Accelerate Framework on iOS][accelerate-link].  The Accelerate Framework implements the Fast Fourier Transform (FFT) as a part of the resource and speed optimized transforms available via the SDK for vDSP (Digital Signal Processing).

### The Accelerate Framework ###

Apple's [Accelerate Framework][accelerate-link] is a low level C API that implements several math operations to run quickly and efficiently on Apple hardware.  The library is available on both iOS and OSX and can be used for image processing, signal processing, matrix math and vector operations.  The specific application that I am experimenting with here is the Fast Fourier Transform (FFT) as is applies to digital signal processing.  

Last week, I created a simple [CoreAudioMixer][github-link] app that implemented AUGraph from the CoreAudio AUToolbox API to playback a couple audio tracks together and allow setting the playback volume of each track.  I thought it would be great to have some visualization of the audio as the track plays and began to investigate options.  I also wanted an opportunity to use the Accelerate Framework, so the FFT and audio spectrum visualization seemed like a great fit.

The CoreAudioMixer app processes input data from two separate audio tracks that are loaded from file: guitar and drums. The tracks are stored in a buffer and sent to output in the AURenderCallback method.  The callback method processes audio data in buffered segments of 512 frames.  The data is processed in real time and sent to the output as the audio plays.  

### The FFT (Fast Fourier Transform) in AURenderCallback ###

In order to analyze the audio frequency spectrum of the audio as it is played back, the Fast Fourier Transform can be used to transform the time based audio data into frequency spectrum buckets with intensity levels.  This calculation needs to be performed across an array of 512 frames of data, so it has to happen quickly.  The vDSP library in the Accelerate Framework provides the calls we need.  [vDSP_fft_zrip](https://developer.apple.com/library/prerelease/ios/documentation/Accelerate/Reference/vDSPRef/index.html#//apple_ref/c/func/vDSP_fft_zrip) is used to perform a forward transform on the vector of data and generate the real and imaginary component output.  The resulting array of data points is stored as a snapshot of 256 points as the audio plays.  The CoreAudioManager class maintains the window of data and allows the consumer (UI) to request an updated set of data whenever appropriate.

Below is the code used to perform the FFT on the audio buffer in the AURenderCallback method:

```objective_c
static void performFFT(float* data, UInt32 numberOfFrames, SoundBufferPtr soundBuffer, UInt32 inBusNumber) {

    int bufferLog2 = round(log2(numberOfFrames));
    float fftNormFactor = 1.0/( 2 * numberOfFrames);
    
    FFTSetup fftSetup = vDSP_create_fftsetup(bufferLog2, kFFTRadix2);
    
    int numberOfFramesOver2 = numberOfFrames / 2;
    float outReal[numberOfFramesOver2];
    float outImaginary[numberOfFramesOver2];
    
    COMPLEX_SPLIT output = { .realp = outReal, .imagp = outImaginary };
    
    //Put all of the even numbered elements into outReal and odd numbered into outImaginary
    vDSP_ctoz((COMPLEX *)data, 2, &output, 1, numberOfFramesOver2);
    
    //Perform the FFT via Accelerate
    //Use FFT forward for standard PCM audio
    vDSP_fft_zrip(fftSetup, &output, 1, bufferLog2, FFT_FORWARD);
    
    //Scale the FFT data
    vDSP_vsmul(output.realp, 1, &fftNormFactor, output.realp, 1, numberOfFramesOver2);
    vDSP_vsmul(output.imagp, 1, &fftNormFactor, output.imagp, 1, numberOfFramesOver2);
    
    //Take the absolute value of the output to get in range of 0 to 1
    vDSP_zvabs(&output, 1, soundBuffer[inBusNumber].frequencyData, 1, numberOfFramesOver2);
    
    vDSP_destroy_fftsetup(fftSetup);
}
```

### The FrequencyView Interface ###

In order to visualize the audio spectrum data in some way, I setup a simple FrequencyView class to simply show bars at each of the frequency buckets provided as output from the FFT.  The view is written in Swift and has an array of float values that can be updated.  Whenever the values are updated, the view adjusts the bar views to the appropriate height on screen.

I setup a simple NSTimer in the main ViewController class that performs a request for new spectrum data every 0.01 seconds.  The UIViews inside the FrequencyView are then updated based on the new data:

```swift
    func timerTick(sender:NSTimer?) {
        if audioManager.isPlaying() {
            //Get the frequency data from the audio manager and show on horizontal bar graph
            var size:UInt32 = 0
            let frequencyData = audioManager.guitarFrequencyDataOfLength(&size)
            let frequencyValuesArray = Array<Float32>(UnsafeBufferPointer(start: UnsafePointer(frequencyData), count: Int(size)))
            
            //Sanity check for expected 256 length
            if frequencyValuesArray.count == 256 {
                frequencyView.frequncyValues = frequencyValuesArray
            }
        }
    }
```

I've implemented the user interface components in Swift.  Swift provides a convenient way to make a Float array out of the buffer pointer that is passed back by the Objective-C class via the UnsafeBufferPointer.  This allows the data to be captured and consumed in a simple way by the Swift views.

### The Source ###

You can check out [CoreAudioMixer][github-link] the source code on [GitHub][github-link].

For more about CoreAudio, visit the iOS [CoreAudio Apple Developer Library](https://developer.apple.com/library/ios/documentation/MusicAudio/Conceptual/CoreAudioOverview/CoreAudioEssentials/CoreAudioEssentials.html).

[github-link]: https://github.com/welbesw/CoreAudioMixer
[accelerate-link]: https://developer.apple.com/library/prerelease/ios/documentation/Accelerate/Reference/vDSPRef/index.html#//apple_ref/doc/uid/TP40009464