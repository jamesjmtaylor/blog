---
title: Appium
date: '2024-10-07T07:36:29-07:00'
---
![Appium AI](/img/blog/appium-sm.jpeg)

I recently had to develop a test framework for a client.  One of the requirements was that the tests had to be able to interact with elements outside of the app in order to provide a full "end-to-end" test suite.  This alone ruled out native Espresso tests, which I had used in the past, because Espresso is only capable of interacting with views within the app.

After a short stint of research I found Appium to be pretty much the industry standard for these kinds of tests.  In this post I plan to take you through the setup steps that I undertook to write and run my own Appium tests.  

The first step was to install [Appium Inspector](https://github.com/appium/appium-inspector/releases). Appium inspector provides a graphical user interface with which to record interactions and assertions with the visual elements presented on your device, including elements outside of your app's process.  This is particularly useful if your app uses an OAuth2.0 Authorization Framework.  

In order to connect to your device you'll also need to download appium, install its drivers, and run the program in your terminal.  This can be done with the following commands:

```
brew install appium &&appium driver install xcuitest &&appium driver install uiautomator2  &&appium
```

Once you have In Appium running in your terminal you will need to specify the Capabilities in the Appium Inspector's JSON Representation block. These include listing which app package to test, which activity to launch, with which automation driver, and so on.  Note that if you specify an applicationIdSuffix in your `build.gradle` you will need to exclude that suffix from the `appActivity` capability. After creating your capabilities set make sure to click the save icon.

Once the Appium Inspector session is started you can record the commands that you execute by tapping on the camera icon in the top menu.  The recorded instructions will appear in the “Recorder” tab.  If a view doesn’t reflect what the device is showing, click the “Refresh Source & Screenshot” button in the top menu.
