---
layout: post
title:  Multi-Robot Exploration and Map Merging
date:   2021-03-21 12:00:00 +0300
image:  frontier_edges.png
tags:   SLAM, Frontier Exploration, Map Merge, ROS, C++
---

# Project Overview
In this project, my goal was to use multiple robots to autonomously explore an environment and create one global merged map comprised of each robot's individual map. This was be achived with or without knowledge of the robot's initial positions. Frontier exploration was implemented as the autonomous navigation algorithm and map merging was exectued by modifying the <a href="http://wiki.ros.org/multirobot_map_merge" target="_blank" rel="noopener noreferrer">multirobot_map_merge</a> node. Have a look at the code on my <a href="https://github.com/gingineer95/Multi-Robot-Exploration-and-Map-Merging" target="_blank" rel="noopener noreferrer">GitHub</a>.

Below is a link to a video (__x speed) of the project.

** insert viedo here **
<!-- <a href="http://www.youtube.com/watch?v=0IDe7L2YoR4" target="_blank" rel="noopener noreferrer">
![computer_vision.png](http://img.youtube.com/vi/0IDe7L2YoR4/0.jpg)
</a> -->

## Frontier Exploration and SLAM
In order for the robots to autonomously explore an environment, I implemented frontier exploration from scratch. By manipulating the occupancy grid generated from <a href="http://wiki.ros.org/slam_toolbox" target="_blank" rel="noopener noreferrer">slam_toolbox</a>. I was able to find all the frontier edges and then group the edges into seperate frontier regions via <a href="http://web.archive.org/web/20200218053936/http://robotfrontier.com/frontier/detect.html" target="_blank" rel="noopener noreferrer">this algorithm</a>. For each frontier region, I found the region's centroid. Lastly, I determined which centroid was closest to the robot's current position and chose that centroid to move to. This process repeats until all areas of the map are explored. 

<div align="center">Frontier edges are open cells adjacent to unknown cells, marked with an “x” below. </div>
<p align="center">![]({{ site.baseurl }}/images/frontier_edges.png)</p>

<div align="center">Frontier regions are groups of adjacent frontier regions, marked with different colors below. Each region's centroid is also marked.</div>
<p align="center">![]({{ site.baseurl }}/images/frontier_regions.png)</p>
<p align="center">
  <img width="460" height="300" src="{{ site.baseurl }}/images/frontier_regions.png">
</p>

Below is a video of frontier exploration working on one robot both in simulation and on an actual Turtlebot3. 

** insert video **

## Map Expansion
Before being able to merge the robot's maps, I had to manipulate the maps. The multirobot_map_merge node that I used was originally written for gmapping and produced maps with the same size and origin for all robots. However, I decided to utilize slam_toolbox instead of gmapping, This meant that I had to expand the maps I recieved from slam_toolbox and resize them so that all robot maps had matching origins and sizes. 



## Map Merging

** insert flowchart **