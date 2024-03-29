---
title: WWDC 17
date: 2017-06-10T13:28:32.000Z
---

![WWDC](/img/blog/wwdc17.jpg)

One of the many benefits of working at Steelcase is that each year every developer is allotted $5000 to attend conferences.  This year I entered the Apple lottery for WWDC tickets and I won!  Over the course of 5 days I learned an immense amount about Swift 4, Xcode 9, iOS 11, Natural Language Processing, Computer Vision, and Machine Learning with CoreML.  What follows is a massive brain dump of all the notes and highlights that I took during the conference.

## Swift 4

* Strings are now a collection of characters, enabling methods calls like myString.first() or myString\[myString.startIndex].  Strings also support slicing into substrings which must be cast back to String explicitly. Finally, multi-line strings in the IDE (as opposed to the automatic IDE word wrapping) are possible by using triple quotes (""") at the beginning and ending of the string. (read more here https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html)
* Objects can now be declared as KeyPaths.  Keypaths allow you to declare run-time closures on the objects which now implement .observer().  (read more here https://developer.apple.com/documentation/swift/key_path_expressions) 
* Classes and Structs support single line JSON encoding and decoding by implementing the [Codeable protocol.](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)
* The 'private' access modifier now does exactly the same thing as 'fileprivate'. 
* Mutable collections now have a .swapAt(\_:,\_:) function.

## Xcode 9

Swift refactoring in the Xcode IDE is now on par with refactoring in Android Studio.  This includes:

* Renaming methods and classes throughout the project.
* Adding and changing method arguments throughout the project.
* Extracting hard coded values, strings, or colors to external variables.

Build times have been reduced by simultaneous indexing and building and bridging header file templating. Build time for PA from a cleaned build folder to app start for Xcode 8 is 121 seconds. For Xcode 9 it is 117 seconds. Successive launches of the app take 8 seconds for Xcode 8 and 4 seconds for Xcode 9.

* In order to turn on the new build process in the menu bar select **File** > **Workspace Settings** > **Build System** > **New Build System (Preview)**.  Do it for both Share and Per-User settings.

New simulators have been added. These simulators have manipulable hardware buttons, dynamic resizing, and support multiple simulators run in parallel.

Wireless debugging is possible with phones running iOS 11+ and that are on the same wireless network as the mac running Xcode.  To connect, make sure the device is plugged in first, then in the menu bar select Window > Devices and Simulators > Select your Device > and check Connect via network.  You can now unplug the device and debug it wirelessly over the network.  Once setup has been completed once it does not need to be repeated.

In the Xcode view debugger you can now choose to show clipped areas for enhanced visual debugging.

You can now wrap logical groups of UI tests commands into groups with Activities (i.e. XCTest.RunActivity(...))

You can now have Xcode automatically take screenshots during UI tests with XCUIElement.screenshot() and SCUIScreen.screenshot().  Both are deleted if the test passes unless you explicitly dictate otherwise.

XCUI.waitForElement() is now recommended over explicitly calling sleep()

## iOS 11

* The NavController.NavBarTitle should now default to prefersLargeTitles. 
* All of the iOS phone controls and settings are now contained on a single screen, accessible by dragging up from the bottom of the screen.
* All of the iOS lock screen notifications are accessible even when the phone is unlocked by dragging down from the top of the screen.
* SafeArea has replaced topLayoutGuide and bottomLayoutGuide for defining the area that will not be obscured by the default navigation bar and footer bar.
* leadingSwipeActionsForRowAtIndexPath and trailingSwipeActionsForRowAtIndexPath APIs have been exposed to TableView, enabling developers to create their own custom implementations.
* UIDocumentBrowserViewController has been released, allowing developers to create screens dedicated to file system manipulation on iphones, tablets, macs, etc.
* Notifications can either be Local, Remote, Silent (direct from the server to the app), or Hidden (displayed, but abbreviated on screen lock).
* Remote Service Extensions allow you to download media from a url embedded in a notification message for display in the notifications window when you can't embed the media in the push notification directly.
* Extensions themselves are not intractable, and need to be overlaid with media buttons in order to trigger (i.e. youtube video)

## Privacy

To uniquely identify an app instance use "let id = UUID()" (constructor natively supported by Foundation)

As of iOS 11 it is now necessary to define "When in Use" and "Always" permission strings in the info.plist file.

Apple assigns each device 2-bits that are uniquely referenced to each device and are accessible at run-time through the apple DeviceCheck API.

## NLP

* Additional Natural Language Processing APIs have been exposed in Foundation through the [NSLinguisticTagger class.](https://developer.apple.com/documentation/foundation/nslinguistictagger).
* Included are Language Recognition, word/sentance/paragraph/document tokenization, part of speech tagging, and lemmatization.
* Lemmatization is the most important concept, reducing conjugated words to their base nature (i.e. 'hiking', 'hiker', 'hikable' all matchable with the word 'hike').
* Exposure of the .dominantLanguage method also allows for localized NLP.

## Vision

The Vision API enables Image Registration (stitching images together), Rectangle Recognition, and Object Tracking.  It supplements existing computer vision pre-processing libraries in CoreImage and AVCapture as a high quality, but slower, alternative.  

There are three parts to implement Vision

1. Create the request
2. Run the request
3. Use the results.

Vision supports CIImage, CGImage, NSURL, and NSData formats.  Make sure to use a completion handler for requests.

## CoreML

* CoreML is an Apple provided Machine Learning API that allows you to import pre-trained Artificial Intelligences directly into Xcode.  Importing is as simple as dragging and dropping a CoreML model into the Workspace directory pane in Xcode.  Once that's done, right click on the file in Xcode to see the model's constructor, as well as the inputs and outputs for an instantiated object's .predict() method.
* As of this writing Apple offers four pre-trained models ranging in size from 25mb to 550mb, all focusing on recognition of objects and scenes in an image.  They're available at https://developer.apple.com/machine-learning/
* A new data type, MLMultiArray, has been created as an advanced input type specific to CoreML that allows audio/video/image collections.
* CoreML functions best with other existing libraries to pre-process input.  For example, NLP to tokenize and lemmatize words that can then be fed into a CoreML model for sentiment analysis (i.e. this is a happy text message vs this is a text sad message), or Vision to recognize sticky-note rectangles which it corrects for skew and then feeds into a CoreML model for handwritten text parsing.
* CoreML handles context switching between CPU and GPU processes for you, optimizing AI processing speed automatically.

To allow use of models trained outside of CoreML (i.e. Keras, Caffe, scikit-learn, libsvm, and XGBoos) execute [pip install -U coremltools](https://pypi.python.org/pypi/coremltools) in the command line.
