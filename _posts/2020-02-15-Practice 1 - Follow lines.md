---
layout: post
title: Followlines
description: Descriptions of the steps followed of the design and develop of the followlines
author: Roberto Gallardo Cava
---

## Introduction

The objetive of this practice is to develop an algorithm that is responsible for the automatic piloting of a formula one in a given circuit following a line drawn on the track.
The line is a red line and is located in the center of the track to facilitate the work of detection. In this practice, it will be used image detection techniques and PD controllers
to find a solution that satisfies the problem.



![sample post]({{site.baseurl}}/images/inicio.PNG)


## First steps

The first task done was the image thresholding, just holding the red pixels of the line we have to track to make it easy to our formula one.

## First version


Para el desarrollo de esta versión se ha utilizado un único controlador que mide la cantidad de píxeles que detecta una linea especifica de la imagen despues del tra



### Lights improvements

3 lineas robustez
ligera formula de la velocidad

<iframe width="640" height="400" src="{{site.baseurl}}/images/v1.mp4" frameborder="0" allowfullscreen></iframe>
{: .video}