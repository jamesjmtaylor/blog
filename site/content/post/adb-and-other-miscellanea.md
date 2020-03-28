---
title: ADB and other Miscellanea
date: '2020-04-01T08:11:25-07:00'
---
At Nautilus I’ve been hard at work on a new line of Google-Certified OEM (Original Equipment Manufacturer) touchscreens for our fitness equipment.  This work has meant that I’ve had to become much more familiar with ADB (Android Debug Bridge) specifically and the Android Operating System in general. 

The first step when developing on Single Board Computers (SBCs) was to establish a connection.  This could be done either over USB or a wireless network.  Once that was done I confirmed the connection by executing \`adb devices\` in the terminal and confirming the presence of the device name.  The next step would be to install the application.  This was accomplished by executing \`adb install /path/to/apk\`.  To ensure that no typos were made when transcribing the path, I drag and drop the apk itself into the terminal window.  This transcribes the absolute path into the terminal window.  

If an apk needed to be extracted from the device it could be accomplished by executing \`adb pull /path/to/apk \`. Any time you pull an apk from a device you strip the Google Play signature from it. The \`adb pull\` command also works if the application is an AAB (Android Application Binary).  The difference is that this will generate multiple .apk files.  If you need to subsequently install the apks back onto the device you can do so all at once by running \`adb install-multiple apk1 apk2 apk3\`.  It is important to note that you must include all the apks that you pulled, otherwise you risk creating a corrupted .aab file.

On our SBCs it was initially necessary to circumvent the system-level permissions in order to access the device’s serial bus.  This was because USB (Universal Serial Bus) access was a user-level permission, but general serial bus access was a system-level permission and not normally accessible to the user.  Because Android is at its heart (technically its kernel) a Linux based OS, we could accomplish this by escalating our ADB permissions to root and using \`shell\` to execute privelaged requests. This was done with the following commands:

\`\``

adb root

adb shell setenforce 0

adb shell chmod 777 /dev/ttyS4

\`\``

While on the topic of permissions, a distinction should be made between Internal and External storage.  In android these terms can describe two different things.  They can refer to internal and external memory (i.e. SD card) versus the device’s non-removable hard-drive space.  They can also refer to the internal app memory (sandboxed to the app's process) versus everything external to that (i.e. the user's photo's directory). Under either definition you need user-level permissions for external memory access.

Communications between android processes and activities normally occur through intents.  Occasionally it was necessary to trigger our applications from the terminal with specific intents and intent extras.  We accomplished this with commands like the one below:

\`\``

adb shell am start -a android.intent.action.MAIN --es machineType "BIKE" -n com.nautilus.app/com.nautilus.ui.view.activity.MainActivity

\`\``

The command above is an example of launching our application and communicating that it should default to “bike” mode.  Intents can serve a wide number of purposes in Android.  Deep links are created by launching intents for the specified package and activity.  Files can be shared as binary streams contained in an intent's EXTRA_STREAM. You can start services through intents as background services. You can pair pending intents with notifications to create foreground services. Finally you can create an intent service that receives an intent, executes, and then finishes automatically. The first two options also allow you to bind and unbind the service from other objects. Doing this allows the objects to call the services public methods directly instead of relying on a broadcast manager. 

It was sometimes necessary to extract crash logs from the devices.  If the application crashes you can export the crash log by using \`adb logcat > ~/Desktop/logcat.txt\`.  This will drop the most recent stack trace into a text file on your development machine’s desktop.  Logcat message format is "date time PID-TID/package priority/tag: message". Priority is either V/D/I/W/E and stands for Verbose/Debug/Info/Warning/Error. TID stands for "Thread Identifier" and PID stands for "Process Identifier".  They can be the same if there is only one thread in the process.

I hope you’ve found this article useful. Let me know in the comments!
