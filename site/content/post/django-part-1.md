---
title: Django (part 1)
date: '2020-06-01T00:00:00-07:00'
---
<img style="float: left; margin:0 1em 0 0; width: 33%" src="/img/blog/django.png"/> Recently a startup that I work part-time for asked that I transition to back end development.  They were happy to have me help, even after hearing my disclaimer that I had little experience with the framework that they were currently using, Django.  Eager to learn, I accepted the offer and have spent the past month learning the ropes and implementing features here and bug fixes there.  This is the first post in a two part series.  It will cover my philosophy on how I approach working in a completely new framework and how I account for billable hours.  The second will be on the lessons I've learned after diving into the deep end of developing a Django brown-field project.  I also have a [tangentially related post](/post/aar-pt-2-python/) in my old AAR series specifically about python development.

Starting work on a brown-field project in a framework that you've never used before presents two challenges.  The first is how best to approach learning the new framework.  The second is how to account for the work when billing.  I'll tackle the first question now.  

I find that just jumping in and replicating the existing patterns to be the most efficacious for rapid development.  It's only when I encounter a novel situation that doesn't have a precedent in the existing codebase or doesn't behave as it does in other instances that I'm forced to take a step back.    If it seems like a simple blunder on my part I generally just execute a quick Google search, explore the top three results, and move on from there.  When it seems to be a more fundamental concept that's eluding me though I'll
