---
title: Information Theory in IoT (Part 2)
date: '2020-10-01T09:19:50-07:00'
---
<img style="float: left; margin:0 1em 0 0; width: 33%" src="/img/blog/huffman.jpg">

This is the second part in the Information Theory in IoT series and will discuss converting natural data into independent identically distributed (IID) values as well as Huffman encoding and error correction bits.  The photograph is of of David A. Huffman, developer of Huffman Encoding.  Before we get into Huffman encoding though we need to discuss making the natural data IID.

In this article we'll address the compression of a single metric, calories per minute.  At Nautilus this metric is colloquially called "Burn Rate".  I chose burn rate because in our flagship app, JRNY, we graph the burn rate as a function of time.  This will allow us to visually compare the graphs before and after the compression, making degradation of data fidelity fairly obvious.  

Two of the standard methods for IID in an application agnostic manner are the [Discrete Fourier Transform](https://en.wikipedia.org/wiki/Discrete_Fourier_transform) and the [Discrete Cosine Transform](https://en.wikipedia.org/wiki/Discrete_cosine_transform).  The first is usually applied to a particular metric over time, while the second is applied to a two dimensional array of data, such as an image.  While these are industry standards, they are by no means the only methods for accomplishing IID.  In this example we'll be using rates of change, first because of the ease of explanation and second because even with just rate of change data it should be more than enough to demonstrate the vast compression benefits of applying Information Theory to IoT data sets.

The array below is a simplified sample of burn rate data, with a sample taken once per second.

`[10,16,14,15,13,13,12,11,13,12,13,14,14,14,13,16,15,14,15,15,15,16,16,16,17,17,18,17,19,21]`

The 30 values above, stored as 16 bit integers, would occupy  480 bits of space.  To make the data more IID, we can store the initial value and then just the rates of change.  This gives us the data below:\
`[10,6,-2,1,-2,|0,-1,-1,2,-1,1,1|,0,0,-1,3,-1,-1|,1,0,0,1,0,0,1|,0,1,-1,2,2]`

The Huffman encoding algorithm involves taking values from a set of data and assigning each value a leaf in a binary tree.  To maximize the data savings, the most common values are placed closer to the root node of a tree.   The simplest rules for generating a Huffman tree use a priority queue for assigning leaf locations, with lowest probability nodes given highest priority.  In a queue FIFO (first in, first out) priority is applied.  But in a priority queue this rule is only used for breaking ties in priority.  In our example the highest priority is a tie between the values 3 & 6.  From there the following rules are applied<img style="float: right; margin:1em 0 0 1em; width: 50%" src="/img/blog/Huffman_coding_visualisation.png">

1. Create a leaf node for each symbol and add it to the priority queue.
2. While there is more than one node in the queue:
   * Add the two nodes of highest priority from the queue
   * Create a new internal node with these two nodes as children and with probability equal to the sum of the two nodes' probabilities.
   * Add the new node to the queue.
   * Repeat until there are no nodes remaining in the priority queue
3. The remaining node is the root node and the tree is complete.

<img style="float: left; margin:0 1em 0 0; width: 50%" src="/img/blog/huffman_br_tree.png">This algorithm is visually depicted above, while the application of those rules is depicted to the left.  Each digit is showed as an alternating red or blue set of bits to assist with visually parsing the data.  The total number of bits is 71.  Adding the 16 bits for the unsigned integer encoding of the initial burn rate value, the application of IID and Huffman encoding has reduced the number of bits from 480 bits to 87, for a reduction in size of 82%.  Another way to look at it is that with 30 integer values represented by 71 bits, each value requires only 2.36 binary digits to be represented, as opposed to the 16 of the original, uncompressed encoding.

Now there is one huge asterisk for this compression ratio.  That is that this Huffman encoding example is for a very specific set of data.  In order to be able to encode and decode a more generalized set of data we would need to capture all the changes in burn rate for all of the workouts recorded.   Given the wide spectrum of possible values we would want to cap the amount of change between data points in order to limit the total number of values that we would need to generate codewords for.  Doing so of course limits the fidelity of the data.  But this limitations are relatively minor compared to the fact that we can now store up to five times more workouts than we could previously.

Image credit to <https://www.cise.ufl.edu/~manuel/huffman/press.release.html>
