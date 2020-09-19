---
title: Information Theory in IoT (Part 2)
date: '2020-09-19T09:19:50-07:00'
---
<img style="float: left; margin:0 1em 0 0; width: 33%" src="/img/blog/nasir.png">

This is the second part in a three part series and will discuss converting natural data into independent identically distributed values.  The photograph is of of Nasir Ahmed, developer of DST, or the [Discrete Cosine Transformation](https://en.wikipedia.org/wiki/Discrete_cosine_transform).  This function has been fundamental in the field of data compression, including High Definition Television (HDTV), audio encoding, a JPEG image compression. By using DST, computers can achieve high-fidelity data compression ratios of 8:1 to 14:1, and ratios of up to 100:1 for acceptable-quality content.  

For the sake of the simplicity of this article, we'll only address the compression of a single metric, calories per minute.  At Nautilus this metric is colloquially called "Burn Rate".  I chose burn rate because in our flagship app, JRNY, we graph the burn rate as a function of time.  This will allow us to visually compare the graphs before and after the compression, making degradation of data fidelity fairly obvious.  



Image credit to <https://en.wikipedia.org/wiki/File:Nasir_Ahmed.png>
