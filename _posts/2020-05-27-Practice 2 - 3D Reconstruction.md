---
layout: post
title: 3D Reconstruction
description: Development of the 3D Reconstruction
author: Roberto Gallardo Cava
---

## Introduction

The objetive of this practice is to develop an algorithm to be able to reconstruct a 3D scene, with objects of various sizes and color, and represent it in a point cloud.
The used system is a canonical stereo formed by two cameras.


![start image]({{site.baseurl}}/images/3d_reconstruction.png)

******************************************************************************************************************************************************************************************************************
<br>
## First steps

Like any good computer vision problem, the first task was the thresholding. The colour space, in both images will be change to grayscale, to later apply a Canny filter, to get the borders
of the objects that it will be use to reconstruct it.

![no dilate left]({{site.baseurl}}/images/left_no_dilate.jpeg)

******************************************************************************************************************************************************************************************************************
<br>
## Main task

Once the reference points in both images have been obtained, the next job is the matching. It will start from a characteristic point in the left image, and with the help of the 
epipolar geometry, it will match the homologous point in the right image, getting the same point, in pixel coordinates in both images.

#### Epipolar restriction

Serching the homologous point in the entire image would be very expensive, so to speed up this process, this restriction will be very useful.

The epipolar restriction says that the homologous point of one point(P1) in the image(I1), is found on the epopolar line(L2) that associate the point P1 with the other image(I2).

* The pixels are detected now by three contiguous lines to make this version more robust.
* A formula that calculates the speed of the formula one has been added.

In the following video you can see the results of this version:
<iframe width="640" height="400" src="{{site.baseurl}}/images/v1.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}

******************************************************************************************************************************************************************************************************************
<br>
## Intermediate version

The first relevant change that should be discussed, was the division of the general controller created into the straight controller and the curve controller, giving to the fist one a greater speed and the 
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
one improved significantly in the line tracking. The areas of the states were readjusted too.

The next objective was to try to properly approximate the speed formula, so far, the formulas have been estimated by trial and error. At this point, the speed depends of two constans, wich will be multiplied by the displacement and the previous
displacement respectively, to finally be added. Each mode, not counting heavy curves controller, has an upper limit and a lower limit, and it has been estimated that for the calculation of
the speed, the value of the current displacement is four times greater than the previous displacement. This ends up resulting in a system of three equations with two unknowns, an overdetermined
system. This system was solved by using least squared, generating values that can be seen in the following graph.
![image regions]({{site.baseurl}}/images/grafica_velocidad.png)
With this new way of calculating speed, the changes of speed are much smoother and natural.

Finally, after the relevant adjustments in the calculation of rotation after having changed the speed of the algorithm, a very stable version has been developed as can be seen in the following video.
<iframe width="640" height="400" src="{{site.baseurl}}/images/v3.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}

******************************************************************************************************************************************************************************************************************
<br>
## Discarded ideas

This section will contain the ideas that were tried to implement but didn't give the expected results.

* High speed version: In one version developed, the formula one reached speeds between 25 and 30, but was extremely unstable, finishing few races, and very rarely following the line, which is the main reason
of the practice.
* Reference lines at different heights: In the project present, the reference lines are together, the idea was to separate them at different heights, to cover more information of the image. Having three lines, long, medium and short distance.
This idea was tried to implement in a very advanced phase of the project, so after attempts that did not give the expected results and the amount of changes involved in the project, the idea was finally discarded.

