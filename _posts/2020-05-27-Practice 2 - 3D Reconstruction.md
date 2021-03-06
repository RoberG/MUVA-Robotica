---
layout: post
title: 3D Reconstruction
description: Development of the 3D Reconstruction
author: Roberto Gallardo Cava
---

## Introduction

The objetive of this practice is to develop an algorithm to be able to reconstruct a 3D scene, with objects of various sizes and colours, and represent it in a point cloud.
The used system is a canonical stereo formed by two cameras.


![start image]({{site.baseurl}}/images/3d_reconstruction.png)

******************************************************************************************************************************************************************************************************************
<br>
## First steps

Like any good computer vision problem, the first task was the thresholding. The colour space in both images will be changed to grayscale, to later apply a Canny filter to get the borders
of the objects that will be used to reconstruct it.

![no dilate left]({{site.baseurl}}/images/left_no_dilate.jpeg)

******************************************************************************************************************************************************************************************************************
<br>
## Main task


Once the reference points in both images have been obtained, the next step is the matching. It will start from a characteristic point in the left image, and with the help of the 
epipolar geometry, it will match the homologous point in the right image, getting the same point, in pixel coordinates in both images.

#### Epipolar restriction

Serching the homologous point in the entire image would be very expensive, so this restriction will be very useful to speed up this process.

The epipolar restriction says that the homologous point of one point (P1) in the image (I1), is found on the epopolar line (L2) that associate the point P1 with the other image (I2).

![no dilate left]({{site.baseurl}}/images/epipolar_geometry.png)

There are several ways to calculate the epipolar line. In this practice, there are two calibrated cameras, with the extrinsincs and intrinsics parameters, so, using the formula -R \* t, 
the optical enter (C1) of the left camera can be get. The trick will be project C1 and the interest point that is being analyzed (using the projection matrix, P1 = K \* [RT]) in the image2,
and with two points you can get a straight line. Since this is a canonical system, these epipolar lines will always be parallel.

#### Matching and patches

At this point, a patch from the left image with the point to find will be obtained, using a neighbord radius (after trying different values, the neigbord radius was set to 8 pixels). Patches will be calculated
patches in the right image through the epipolar line, with the same conditions.  In the practice, the homologous point will be searched in a epipolar stripe, not in the main line, because
 there might be slight calibration errors. The patches will be compared using the TM_CCOEFF_NORMED algorithm (using the method *matchTemplate* included in the opencv library),
that returns how similar the two patches are. The threshold was set in 0.95 to be the correlation accepted, being 1 the maximum possible.

#### Triangulation

And finally, once you have all the ingredients, the homologous points in both images and the projection matrix P1 and P2. Using the method *triangulatePoints*
of the opencv library  with these parameters, it will return the 3D position of the homologous point. This method calculates the intersection of the backpropagation lines between both optical
centers and the homologous points. To get the colour of the point, just get the colour at the point(pixel) from one of the original images.


******************************************************************************************************************************************************************************************************************
<br>
## Changes in the triangulation

Due with *triangulatePoints* a satisfactory result was not obtained, the triangulate method was changed using the pure formulas and aplying the geometrics constraints of the problem.

In this image, you can see the final result using the opencv method.

![full triangulatepoints]({{site.baseurl}}/images/full_triangulatepoints.png)


******************************************************************************************************************************************************************************************************************
<br>
## Release version

Following all the steps described in this report and after a period of trial and error adjusting the threshold values ​​and exploring different ways of obtaining certain results, such as the disparity,
a release version has been reached. In these images you can see the final result.

![full triangulatepoints]({{site.baseurl}}/images/final.jpg)
![full triangulatepoints]({{site.baseurl}}/images/planos.jpg)


In the following gif, you can see the complete process.
![3D Recon Gif]({{site.baseurl}}/images/3d_recon.gif)

******************************************************************************************************************************************************************************************************************
<br>
## Discarded ideas

This section will contain ideas that were attempted to implement but didn’t give the expected results.

* High Number of Points: In one version developed, an attempt was made to increase the number of points to obtain a much richer map using a dilation, a morphological operation, that returns
a considerable higer amount of interest points. The speed of the 3D representation highly decreased mostly due to hardware and software limitations, so eventually the idea was discarded. In this image
 you can see the results of the filtering and the 3D plots, with 25800 points without colour (not implemented in this version). Bowser's head and the box are clearly distinguishable.

![dilate left]({{site.baseurl}}/images/left_dilate.jpeg)

![25000 points]({{site.baseurl}}/images/25000_points.jpeg)

* Overfit model: There is a version in which the matching is not done over the entire epipolar line, but is done over a maximum displacement threshold. This idea was discarded because it generates
an overtifit model, because you need a prori knoledge of the image disparity.

