---
layout: post
title: Merge or cluster Superpixels with RAGS and Python.
image: 8.jpg
date: 2019-09-08 17:00:20 +0200
tags:
categories: Tutorial 3DImages
---
In this post I am going to explain a very interesting technique to use along with the segmentation methods to have better self defined areas... or volumes (in a future;) which probably fit better to our needs.

#### Introduction: Segmentation in images
One of the traditional methodologies to work with images is pixel-oriented, it is based on modifying interacting with the pixels, it difers from image processing based on the frequency domain, treating images as signals. Another approach is working with groups of pixels instead of individual pixels, the superpixels. So we can define a superpixel as a group of pixels, but they are not randomly grouped! they need to have something in common, for example their position, they are located near of each other.

The action of creating superpixels is called segmentation of the image. In this post I am not going to go into detail about it, I think there are many blogs and tutorials where they explain these methods. You can checked some of the ones where I learnt the fundamentals to start with my project.

if you are new to the topic the first link has to be:
(https://towardsdatascience.com/image-segmentation-using-pythons-scikit-image-module-533a61ecc980)
where you can read more details about segmentation methods and how superpixels fill in the cathegory of the unsupervised ones.

In this link (https://www.pyimagesearch.com/2014/07/28/a-slic-superpixel-tutorial-using-python/) a basic introduction to the SLIC method in Python is being given. I find the author knows very well how to explain and be entertaining at the same time. 
     

#### Merging/Cluster your superpixels
We got to the point of the post, you may wander why shall we be interested in loosing segmentation by merging our superpixels, why not just do less segmentation since the beginning. Well you will understand better at the end but the key point is that the unsupervised segmentations algorithms do not divide the image as a human would do. For example if you apply them to the image of a hand in a black background, they will explit the hand in several parts (the number will depend upon the parameter of segmentation stablished). Grouping the superpixels with certain criteria could lead to a right segmentation of the whole hand.

I hope I have convinced you enough that we are going to be interested in grouping our superpixels because now I am going to introduce you to the best player of our team for that, the Region Boundary Graphs (RAGs).


#### REGION BOUNDARY GRAPHS
Ok we have our segmented image with way more superpixels that we could want, what if to merge them we draw the mass center of each region and connect it with the centers of the superpixels that share at least one border. We have created a graph where each superpixels is connected with its surroundings. Second step is to set a value for the arc connecting each pair of superpixels, for example it can be the difference between the average color. After this there are parts of the image with greater values where there are shadows or contours; and parts where values are small, more uniform. Then finally, let's set a threshold, all superpixels linked by a value lesser will be turned into a single superpixel.

More information can be seen in the following link (https://vcansimplify.wordpress.com/2014/07/06/scikit-image-rag-introduction/) where one of the developers of the algorithm explains it.


###### Generating the graph: Color vs Border
I find the information of the previous blog and the docs of future.graph of good quality, however I miss some more detail in the differences of the two methods of generating the RAG, for a newbie in image processing as myself was hard in the beginning as that's the reason of this post.

There are two implemented ways of creating the region boundary graph, as well as two different ways of merging the superpixels.
*  



### Happy Coding!


