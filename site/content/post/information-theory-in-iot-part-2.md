---
title: Information Theory in IoT (Part 2)
date: '2020-09-19T09:19:50-07:00'
---
<img style="float: left; margin:0 1em 0 0; width: 33%" src="/img/blog/huffman.jpg">

This is the second part in the Information Theory in IoT series and will discuss converting natural data into independent identically distributed (IID) values as well as Huffman encoding and error correction bits.  The photograph is of of David A. Huffman, developer of Huffman Encoding.  Before we get into Huffman encoding though we need to discuss making the natural data IID.

In this article we'll address the compression of a single metric, calories per minute.  At Nautilus this metric is colloquially called "Burn Rate".  I chose burn rate because in our flagship app, JRNY, we graph the burn rate as a function of time.  This will allow us to visually compare the graphs before and after the compression, making degradation of data fidelity fairly obvious.  

Two of the standard methods for IID in an application agnostic manner are the [Discrete Fourier Transform](https://en.wikipedia.org/wiki/Discrete_Fourier_transform) and the [Discrete Cosine Transform](https://en.wikipedia.org/wiki/Discrete_cosine_transform).  The first is usually applied to a particular metric over time, while the second is applied to a two dimensional array of data, such as an image.  While these are industry standards, they are by no means the only methods for accomplishing IID.  In this example we'll be using rates of change, first because of the ease of explanation and second because even with just rate of change data it should be more than enough to demonstrate the vast compression benefits of Huffman encoding.  



Image credit to <https://en.wikipedia.org/wiki/File:Nasir_Ahmed.png>
