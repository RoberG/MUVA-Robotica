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

The epipolar restriction says that the homologous point of one point (P1) in the image (I1), is found on the epopolar line (L2) that associate the point P1 with the other image (I2).

![no dilate left]({{site.baseurl}}/images/epipolar_geometry.png)

There are several ways to calculate the epipolar line. In this practice, there are two calibrated cameras, with the extrinsincs and intrinsics parameters, so, using the formula -R \* t, 
the optical enter (C1) of the left camera can be get. The trick will be project C1 and the interest point that is being analyzed (using the projection matrix, P1 = K \* [RT]) in the image2,
and with two points you can get a straight line. Since there are a canonical system, these epipolar lines will always be parallel lines.

#### Patching

At this points, it will be obtained a patch from the left image with the point to find, using a neighbord radius (after trying different values, the neigbord radius was set to 8 pixels). It will be calculate
patches in the right image through the epipolar line, with the same conditions.  In the practice, the hologous point will be search in a epipolar stripe, not in the main line, because
 ther may be slight calibration errors. The patches will be compared using the TM_CCOEFF_NORMED algorithm (using the method *matchTemplate* included in opencv library),
that returns how similar are the two patches. The threshold was set in 0.95 to be the correlation accepted, being 1 the maximum possible.

#### Triangulation

And finally, one you have all the ingredients, the homologous points in both images and the projection matrix P1 and P2, as previously was explained. Using the method *triangulatePoints*
of the opencv library  with this parameters, will return the 3D position of the homologous point. This method calculates the intersection of the backpropagation lines between both optical
centers and the homologous points.

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

