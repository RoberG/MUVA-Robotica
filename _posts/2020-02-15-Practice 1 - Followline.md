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



![start image]({{site.baseurl}}/images/inicio.PNG)


## First steps

The first task was the thresholding of the image. It has been necessary change the colour space to HSV, because this space has greater independence of lighting, 
followed by just holding the red pixels of the line that has to be traced to make it easy to the formula one using a colour filter. 

## First version

For the development of this version, a single controller has been used that measures the amount of pixels of a specific line in the image (the image was already treated). It will decided the direction
 to turning the car in order to put it back on the line. It decided the angle of rotation using a simple formula, the number of pixels displaced* constant k.
 
If the formula one loses the reference line, it will go back until it finds it again.

#### Lights improvements

* The pixels are detected now by three contiguous lines to make this version more robust.
* A formula that calculates the speed slightly of the formula one has been added.

In the following video you can see the results of this version:
<iframe width="640" height="400" src="{{site.baseurl}}/images/v1.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}


## Intermediate version

The first relevant change that should be discussed was the division of the general controller created into the straight controller and the curve controller, giving to the fist one a greater speed and the 
second one a greater rotation. Later, the curve controller was subdivided into two controllers, the light curve controller and the heavy curve controller, giving to the first one a light rotation,
with the idea of making smooth transitions between states.

The following image shows the segmentation of the image, which will decide the phase of the formula one. The yellow zone is the area of the heavy curve controller, the blue zone is the area of the 
light curve controller, and finally, the red zone is the area of the straight controller.

![image regions]({{site.baseurl}}/images/regiones.png)

The next change, was the inclussion of the D controller, using the previous frame with the objetive to give more information about the location of the formula one and being able to offer a better response,
redisigning the functions of the speed and rotation calculations.

At this point in the development, special emphasis was placed on a considerable increase in the car speed. After a few hours of work, the values associated to the calculations were fixed, and finally the 
formula one could end the circuit satisfactorily as you can see in the following video.

<iframe width="640" height="400" src="{{site.baseurl}}/images/v2.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}


******************************************************************************************************************************************************************************************************************
<br>
## Release version


The main problem of the previous version was the zigzagging. Despite going faster, the formula one didn't follow the line properly, so the speed was delimited to gain robustness.
Limit speeds for each stage were reset by assigning 10 for the heavy curve controller, 11 to light curve controller and 13 to the straight controller. With these small changes, the formula
one improved significantly in the line tracking.

The next objective was to try to properly approximate the speed formula, so far, the formulas have been estimated by trial and error. At this point, the speed depends of two constans, wich will be multiplied by the displacement and the previous
displacement respectively, to finally be added. Each mode, not counting heavy curves controller, has an upper limit and a lower limit, and it has been estimated that for the calculation of
the speed, the value of the current displacement is four times greater than the past displacement. This ends up resulting in a system of three equations with two unknowns, an overdetermined
system. This system was solved by using least squared, generating values that can be seen in the following graph.
![image regions]({{site.baseurl}}/images/grafica_velocidad.png)
With this new way of calculating speed, the changes of speed are much more smoother and natural.

Finally, after the relevant adjustments in the calculation of rotation after having changed the speed of the algorithm, a very stable version has been developed as can be seen in the following video.
<iframe width="640" height="400" src="{{site.baseurl}}/images/v3.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}

******************************************************************************************************************************************************************************************************************
<br>
## Cosas descartadas

* Coche muy follado
* Puntos de referencia a distintas alturas

