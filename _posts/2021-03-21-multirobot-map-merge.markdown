---
layout: post
title:  Multi-Robot Exploration and Map Merging
date:   2021-03-21 12:00:00 +0300
image:  gaz_unknown_pos.png
tags:   SLAM, Frontier Exploration, Map Merge, ROS, C++
---

# Project Overview:
In this project, my goal was to use multiple robots to autonomously explore an environment and create one global merged map comprised of each's robot individual map. This can be achived with or without knowledge of the robot's initial positions. Frontier exploration was implemented as the autonomous navigation algorithm. Map merging was achieved by modifying the multirobot_map_merge node.

Below is a link to a video (4x speed) of the project. The video starts by showing the bottles and cans being placed, then a quick view of the computer vision, and lastly it displays two different views of the robot in action. 

** insert viedo here **
<!-- <a href="http://www.youtube.com/watch?v=0IDe7L2YoR4" target="_blank" rel="noopener noreferrer">
![computer_vision.png](http://img.youtube.com/vi/0IDe7L2YoR4/0.jpg)
</a> -->

## Frontier Exploration and SLAM

## Map Merging