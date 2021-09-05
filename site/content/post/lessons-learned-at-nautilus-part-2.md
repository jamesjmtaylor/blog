---
title: Lessons Learned at Nautilus (Part 2)
date: '2021-09-05T07:25:40-07:00'
---
<img style="float: left; margin:0 2em 2em 0; width: 31%" src="/img/blog/circuitBoard.jpg"/>

In my two and a half years at Nautilus I learned a tremendous amount about IoT, Hardware, Software, Data Science, and myself in general. Last month I covered the importance of supporting Over-The-Air (OTA) updates from the start.  Today I'll be covering how critical data scientist input can be when starting a project.

When I was hired at Nautilus all of our application analytics were handled by the Adobe Analytics SDK.  The SDK uploaded analytic actions (things a user did) and states (things a user experienced) to S3, where a nightly job transferred the accumulated data into Azure Databricks.   Our product owners could then run queries on the dataset using Microsoft Power BI to answer questions the business might have about our customers in general.  

The problem with this setup was that we didn't have a dedicated data scientist to validate the coherence or completeness of the data.  The mobile development team did it's best to capture every action or state that they thought might be relevant.  But we just couldn't anticipate all the questions the business might have, or the edge cases around the data. 

It wasn't until we acquired a team of dedicated data scientists that the unknown unknowns were revealed.  We realized that we didn't have the data to answer questions like "who's the favorite virtual trainer for bicyclists?"  Such a question had clear business importance because it would allow us to provide our users with more content from that coach and less from the other trainers. But we simply had not anticipated it as a question when creating our analytics collection points.  As a result we missed out on over two years of data before we realized that we needed to collect it.

Answers to other questions had to be painstakingly reconstructed by the data scientist team. In one example, the average length of a treadmill workout had to extrapolated using the timestamps of the workout start analytics action and the workout saved analytics state as associated by the session id.  This required a lot of work in Microsoft Power BI that might have been avoided if the mobile team had simply sent the workout duration of each workout by exercise equipment id. 

You might have recall that I mentioned user edge cases earlier, which brings me to my final point, data validation.  Our product owners were brilliant at their jobs, which was to represent the voice of the customer in the development of the JRNY app.  But because they weren't data scientists they missed some analytics edge cases that seem glaring in hindsight.  







Photo by <a href="https://unsplash.com/@lazycreekimages?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Michael Dziedzic</a> on <a href="https://unsplash.com/s/photos/data-science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
