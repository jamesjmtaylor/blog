---
title: KMM Database layer
date: '2022-08-08T05:49:44-07:00'
---
<img style="float: left; margin:0 0 1em 0; width: 100%" src="/img/blog/sweden.jpg"/> 

This is the second in a multi-part series on Kotlin Multiplatform Mobile.  This entry will cover [configuring SQLDelight and implementing cache logic](https://play.kotlinlang.org/hands-on/Networking%20and%20Data%20Storage%20with%20Kotlin%20Multiplatfrom%20Mobile/05_Configuring_SQLDelight_an_implementing_cache) for the mobile iOS and Android app.  Unlike the tutorial, I'm not using the SpaceX API, which provides public access to information about SpaceX rocket launches.  Instead, I'm using the Army's brand new [ODIN API ](https://odin.tradoc.army.mil/WEG) for a reboot of my WEG iOS and Android applications.  The ODIN API provides in-depth information about a wide array of military equipment.  



Photo by <a href="https://unsplash.com/@springsimon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Simon Spring</a> on <a href="https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
