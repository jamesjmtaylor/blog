---
title: Swift & Kotlin Queue Performance
date: '2023-03-09T06:40:19-08:00'
---
![Queue](/img/blog/queue.png)

This is an expansion on two articles that I did about this time last year: 

* <a href="/post/swift-hackerrank-part-1/">Swift & HackerRank (Part 1)</a>. 
* <a href="/post/swift-hackerrank-part-1/">Swift & HackerRank (Part 2)</a>. 

In those articles I marveled at how much faster Kotlin was compared to Swift in terms of queue performance.  While I quantified the performance in terms of order of magnitude at a specific volume (3 orders of magnitude better performance for 1,000 enques and deques) what I failed to do was generalize the differences in terms of [Big-O notation](https://en.wikipedia.org/wiki/Time_complexity).  That's something I plan to remedy in this blog post.

In order to empirically derive the time complexity I will need to run samples of various sizes, plot the points and fit a formula to each algorithm performance profile.  Based on my understanding of the underlying data structures my hypothesis is that the Kotlin queue will see constant-time enqueuing and dequeuing, that is the thousandth enque (or deque) operation should be just as performant as the first.  For Swift I think enques will be probably be constant as well, but that deques will likely be linear time O(n).  This is because my Swift implementation uses an array, and will need to shift all the elements in the array to the left to fill in the vacated position, while my Kotlin implementation uses a Doubly-linked list (which the Swift standard library lacks). The Apple Developer documentation helpfully confirms this hypothesis concerning [enqueuing](https://developer.apple.com/documentation/swift/array/append(_:)-1ytnt) and [dequeing](https://developer.apple.com/documentation/swift/array/removefirst()), but I'd like to try it empirically in any case.



Photo by <a href="https://unsplash.com/@emilegt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Emile Guillemot</a> on <a href="https://unsplash.com/photos/x5-IRhnJkxI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
