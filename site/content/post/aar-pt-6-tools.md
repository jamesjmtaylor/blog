---
title: AAR pt 6 (Tools)
date: 2018-02-05T13:48:17.031Z
---
<img style="float: left; margin:0 1em 1em 0; width: 33%" src="/img/blog/toolbelt.jpeg"/> 

If you haven't had a chance to read the first entry in the series for context, [you can do so here](/post/after-action-review-aar/). 

Over the course of a professional career you tend to acquire a lot of tools, regardless of the industry.  I wanted to list some of my favorites in this blog entry, as well as some of my lessons learned from each.  I'll go into greater detail for each in the future, but for now I'll just cover some of my major takeaways from each.

* If you are doing any kind of network API interaction, test your all your API calls with [Postman](https://www.getpostman.com/).
* You can get the code equivalent of any Postman call in Java or Swift by clicking on the **Code **button.  If exporting Postman calls as Java use **OKHTTP**.  For swift, use **NSURL**.
* ALWAYS debug network integration through Postman first, then in the simulator with [Charles](https://www.charlesproxy.com/) running (Charles is a Man-In-The-Middle network debugging program)
* Use the application [Network Link Conditioner](http://nshipster.com/network-link-conditioner/) (not normally included on Mac) to simulate a bad connection over localhost.
