---
layout: post
title:  Baxter Recycling Segmentation
date:   2020-12-08 12:00:00 +0300
image:  Baxter_sorting.gif
tags:   ROS, MoveIt!, OpenCV, Python
---
<!-- <div align="center"><em>GIF is playing at 4x speed.</em></div>
<p>&nbsp;</p> -->

# Project Overview:
In this project, my team programmed a Rethink Baxter robot to segment and pick bottles and cans then drop them into separate recycling bins. We used OpenCV to detect and locate the randomly placed bottles and cans, while for the robotic manipulation we utilized MoveIt!. Take a look at the code on my <a href="https://github.com/gingineer95/baxter-recycling-segmentation" target="_blank" rel="noopener noreferrer">GitHub</a>. (Note: the GIF above is playing at 2x speed)

Below is a link to a video (4x speed) of the project. The video starts by showing the bottles and cans being placed, then a quick view of the computer vision, and lastly it displays two different views of the robot in action. 
<!-- <iframe width="750" height="400"
src="https://youtu.be/0IDe7L2YoR4" 
frameborder="0" 
allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" 
allowfullscreen></iframe> -->

<!-- <p>&nbsp;</p>  -->
<!-- <a href="http://www.youtube.com/watch?v=0IDe7L2YoR4" target="_blank" rel="noopener noreferrer">
![computer_vision.png](http://img.youtube.com/vi/0IDe7L2YoR4/0.jpg)
</a> -->

<iframe width="560" height="315" src="https://www.youtube.com/embed/0IDe7L2YoR4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<p>&nbsp;</p>

## MoveIt!
The robot operation was controlled entirely through ROS's MoveIt! library for motion planning and manipulation (mainly the compute_cartesian_path command). After initializing the Move Group Commander for Baxter's right arm, a table is added to the Planning Scene to ensure that the robot does not collide with the table. 

The arm then moves to a position out of the camera's field of view and calls the camera's segmentation service. Once the objects have been located and classified, the arm moves to a predetermined home position over the objects to ensure smooth and predictable motion of the arm (This home configuration was determined after testing).


## Computer Vision
A Realsense camera was used to take real-time video of the objects placed in front of the robot. After calibrating the camera, we used OpenCV and specifically Hough Circles to identify, locate and classify the bottles and cans. The camera segmentation service returns a list with the position and classification of all bottles and cans in the camera's view.

![]({{ site.baseurl }}/images/computer_vision.png)


## Action Sequence
1. Baxter's right arm moves to the home position where the robot is safely above all objects.
2. A livestream of the camera opens that captures, segments and locates all objects on the table. (For this project, we only have bottles and cans)
3. For an object on the table, the robot's head will display either the can image or the bottle image, depending on the classification.
4. Next, Baxter's right arm will move to object's (x,y) coordinate at a safe z height away. This is the same height for bottles and cans.
5. The arm then moves down to the appropriate perch height, depending on classification. (For example, the robot arm will be position further away from the table for bottle, since those are taller than cans).
6. Once safely at the perch height, the ame moves down again. This time aligning the object in between the grippers.
7. Baxter's arm then grasps the object.
8. The arm moves back up to the "safe position"; the same position as step 4.
9. Baxter now moves back to the home position. This step was added to ensure predictable behavior of the robot arm.
10. Depending on the object's classification, the arm will move to the appropriate bin. Baxter's head will also display the recycling image.
11. Once over the bin, Baxter opens its grippers and drops the object. To show that the object has been recycled, Baxter displays the bin image.
12. Steps 3 through 11 are repeated for all objects found.

## Team Members
- Yael Ben Shalom
- Chris Aretakis
- Jake Ketchum
- Mingqing Yuan