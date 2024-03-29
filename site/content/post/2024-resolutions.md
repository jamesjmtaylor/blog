---
title: 2024 Resolutions
date: '2024-01-01T07:45:46-08:00'
---
![2024](/img/blog/2024.png)

2023 proved to be quite the turbulent year.  Of my three[ 2023 new year's resolutions](https://jjmtaylor.com/post/2023-resolutions/), I accomplished two.  Specifically, I completed my Starcraft Rust AI (you can read about it [here](https://jjmtaylor.com/post/starcraft-rust-bot/)) and successfully summited four new mountains, Mt Hood, Old Snowy, Broken Top, and Three Finger Jack. My goals for 2024 are below:

* Update my Worldwide Equipment Guide app to use the new [Odin API](https://odin.tradoc.army.mil/)
* Climb three new mountains.
* Complete Intermediate Climbing School (ICS)

I made token updates to in 2022 Worldwide Equipment Guide app. The 2023 update was a complete rewrite of the Android and iOS apps to use Kotlin Multi-Platform (KMP) as well as a migration to use the new Odin API. 

The OE (Operational Environment) Data Integration Network (ODIN) API is the authoritative digital resource for the Worldwide Equipment Guide (WEG), Decisive Action Training Environment (DATE), and the Training Circular (TC) 7-100 series.  It was first released by the Army in 2019 and has been consistently updated ever since. I was on track to complete the updates to my app to use the new API last year even though they weren't formally a new year's resolution.   Networking was handled by Ktor, database persistence was implemented in SQLDelight, while the user interfaces (UIs) used Jetpack Compose for Android and SwiftUI for iOS. 

Unfortunately a sudden (and extensive) change in the organization of the Odin API meant that I had to basically start from scratch.  So this year I'll focus on just getting the networking and user interfaces working, and worry about data persistence in a subsequent update to the apps.  Once these updates are complete and a large enough percentage of my existing user base is updated, I'll be able to shutdown my home-grown WEG API, saving $72 a year in Digital Ocean hosting fees. 

The second and third goals are repeats of the same goals from 2023.  I wasn't accepted to ICS in 2023 despite a near perfect score on the entrance exam.  This was mostly because there were twice as many applicants as available student slots. When I asked what I could do to make myself a more competitive candidate for 2024 I was told I needed to complete more alpine snow climbs.  So my plan going forward is to scale back my volunteer efforts with the Mazamas (which didn't seem to have an impact on my application) and scale up my climbing efforts.  The Mazamas does keep track of your previous applications to ICS and gives preference to previous applicants.  So my hope is that my previous application combined with a more robust climb resume will be sufficient for me to be accepted in 2024.

I hope you had a great 2023, and that you have an even better 2024.  I'll keep you posted on my progress for this year's resolutions.  Climbing three new mountains will be contingent on which climbs are put on the Mazamas calendar, but I'm optimistic that I'll be able to complete them before the summer.  Starting from scratch with the Odin API certainly seems daunting, but I think focusing on shipping a truly Minimum Viable Product (MVP) will help focus my efforts.  Lastly, my hope is that more climbs will allow me to secure a spot in the 2024 ICS course. Happy New Year!
