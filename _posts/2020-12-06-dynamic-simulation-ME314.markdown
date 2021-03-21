---
layout: post
title:  Dynamics Simulation of a Jack in a Box
date:   2020-12-06 12:00:00 +0300
image:  jack-in-box.gif
tags:   Python, Simulation, Dynamics
---
# Project Overview:
This project entailed simulating a planar multi-body, multi-impact dynamic system for which I chose to model a jack-in-a-box. Given that both the jack and the box have mass and the box also experiences an external force, I calculated multiple rigid body transformations to find the Euler-Lagrange equations, dynamic constraints, and impact update laws. After simulating the system I then animated it as seen in the GIF above (playing at 1x speed). Take a look at the code on my <a href="https://github.com/gingineer95/dynamics_simulation" target="_blank" rel="noopener noreferrer">GitHub</a>. 

## Model Description
In my model, I imagined a "bird's-eye-view" of the system, meaning the system lies on the x-y plane of an imaginary table and the z axis (which experiences gravity) is out of the plane. 

The image below is a drawing of all the system frames. To see just the box or jack frames and their transformations, please look at my <a href="https://github.com/gingineer95/dynamics_simulation" target="_blank" rel="noopener noreferrer">GitHub</a>. There are 11 frames in total including the inertial world frame, the frame at the center of the box, the frames at each corner of the box, the frame at the center of the jack and the frames at each corner of the jack. 

![]({{ site.baseurl }}/images/box_to_jack.png)

## Simulation and Results
I used several rigid body transformations to relate the central box and jack frames to the inertial world frame, relate each corner of an object to its central frame and relate the box and jack to each other. From the transforms, I was able to determine all 16 elastic impact constraints for each of the 4 points on the jack relative to each of the 4 sides of the box. 

After calculating all the necessary transforms and imposing an oscillating external force, I then found the Euler Lagrange equation and impact laws. The final result can be seen in the GIF at the top of the page. Any point on the jack can hit any side of the box and create an impact. As a result of that impact, both the jack and the box will feel an equal and opposite reaction as evidence by the jack bouncing away and the box rotating after each impact.
 