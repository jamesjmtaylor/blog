---
title: Swift & Kotlin Queue Performance
date: '2023-03-09T06:40:19-08:00'
---
![Queue](/img/blog/queue.png)

This is an expansion on two articles that I did about this time last year: 

* <a href="/post/swift-hackerrank-part-1/">Swift & HackerRank (Part 1)</a>. 
* <a href="/post/swift-hackerrank-part-1/">Swift & HackerRank (Part 2)</a>. 

In those articles I marveled at how much faster Kotlin was compared to Swift in terms of queue performance.  While I quantified the performance in terms of order of magnitude at a specific volume (3 orders of magnitude better performance for 1,000 enqueues and dequeues) what I failed to do was generalize the differences in terms of [Big-O notation](https://en.wikipedia.org/wiki/Time_complexity).  That's something I plan to remedy in this blog post.

In order to empirically derive the time complexity I will need to run samples of various sizes, plot the points and fit a formula to each algorithm performance profile.  Kotlin uses a doubly-linked list as its underlying data structure.  My hypothesis is that the Kotlin queue will see constant-time enqueuing and dequeuing, that is the thousandth enqueue (or dequeue) operation should be just as performant as the first. 

My Swift hypothesis is a little more complicated.  If you remember from part 2 of the series, the doubly-linked list implementation was not at all performant, most likely because Swift doesn't perform any memory optimization around linked lists.  The array implementation was vastly more performant, but still underperformed Kotlin by 3 orders of magnitude at 1,000 enqueues and dequeues.  After digging into the Apple Developer documentation the reason became obvious.  [Enqueuing](https://developer.apple.com/documentation/swift/array/append(_:)-1ytnt) uses an array append operation, which occurs in constant time with the benefit of memory optimization like Kotlin. [Dequeuing](https://developer.apple.com/documentation/swift/array/removefirst()) however shifts all the elements to the left to occupy the space vacated by the removed element.  This requires linear (O(n)) time.  So even though it is significantly less performant, we'll need to use the original Swift doubly linked list implementation in order to be able to compare apples to apples (pun not intended).  I'm guessing that while the absolute amount of time to perform each enque and deque will remain greater than Kotlin's, there will be no change in the relative amount of time to betweeth the first enqueue and the thousandth. 

Photo by <a href="https://unsplash.com/@emilegt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Emile Guillemot</a> on <a href="https://unsplash.com/photos/x5-IRhnJkxI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
