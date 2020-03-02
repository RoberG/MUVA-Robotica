---
layout: post
title: Followline
description: Descriptions of the steps followed of the design and develop of the followline
author: Roberto Gallardo Cava
---

## Introduction

The objetive of this practice is to develop an algorithm that is responsible for the automatic piloting of a formula one in a given circuit following a line drawn on the track.
The line is a red line and is located in the center of the track to facilitate the work of detection. In this practice, it will be used image detection techniques and PD controllers
to find a solution that satisfies the problem.



![sample post]({{site.baseurl}}/images/inicio.PNG)


## First steps

The first task was the thresholding of the image. It has been necessary change the colour space to HSV, because this space has greater independence of lighting, 
followed by just holding the red pixels of the line that has to be traced to make it easy to the formula one using a colour filter. 

## First version

For the development of this version, a single controller has been used that measures the amount of pixels of a specific line in the image (the image was already treated). It will decided the direction
 to turning the car in order to put it back on the line. It decided the angle of rotation using a simple formula, the number of pixels displaced* constant k.
 
If the formula one loses the reference line, it will go back until it finds it again.

#### Lights improvements

The pixels are detected now by three contiguous lines to make this version more robust.

A formula that calculates the speed slightly of the formula one has been added.

In the following video you can see the results of this version:
<iframe width="640" height="400" src="{{site.baseurl}}/images/v1.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}