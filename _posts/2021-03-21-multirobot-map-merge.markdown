---
layout: post
title:  Multi-Robot Exploration and Map Merging
date:   2021-03-21 12:00:00 +0300
image:  3x_real_robot.gif
tags:   SLAM, Frontier Exploration, Map Merge, ROS, C++
---

# Project Overview:
In this project, my goal was to use multiple robots to autonomously explore an environment and create one global merged map comprised of each robot's individual map. This was be achived with or without knowledge of the robot's initial positions. Frontier exploration was implemented as the autonomous navigation algorithm and map merging was exectued by modifying the <a href="http://wiki.ros.org/multirobot_map_merge" target="_blank" rel="noopener noreferrer">multirobot_map_merge</a> node. Have a look at the code on my <a href="https://github.com/gingineer95/Multi-Robot-Exploration-and-Map-Merging" target="_blank" rel="noopener noreferrer">GitHub</a>.

Below is a link to a video of the simualed project at 20x speed
<iframe width="560" height="315" src="https://www.youtube.com/embed/6pEU1-0Ax6o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>.

<p>&nbsp;</p>

## Frontier Exploration and SLAM
In order for the robots to autonomously explore an environment, I implemented frontier exploration from scratch. By manipulating the occupancy grid generated from <a href="http://wiki.ros.org/slam_toolbox" target="_blank" rel="noopener noreferrer">slam_toolbox</a>. I was able to find all the frontier edges and then group the edges into seperate frontier regions via <a href="http://web.archive.org/web/20200218053936/http://robotfrontier.com/frontier/detect.html" target="_blank" rel="noopener noreferrer">this algorithm</a>. For each frontier region, I found the region's centroid. Lastly, I determined which centroid was closest to the robot's current position and chose that centroid to move to. This process repeats until all areas of the map are explored. 

<div align="center">Frontier edges are open cells adjacent to unknown cells, marked with an “x” below. </div>
<p align="center">
  <img width="200" height="200" src="{{ site.baseurl }}/images/frontier_edges.png">
</p>

<div align="center">Frontier regions are groups of adjacent frontier regions, marked with different colors below. Each region's centroid is also marked.</div>
<p align="center">
  <img width="250" height="100" src="{{ site.baseurl }}/images/frontier_regions.png">
</p>

Below is a GIF (at 3x speed) of frontier exploration working on one robot both in simulation and on an actual Turtlebot3. 

<p align="center">
  <img width="800" height="400" src="{{ site.baseurl }}/images/3x_real_robot.gif">
</p>

## Map Expansion
Before being able to merge the robot's maps, I had to manipulate the maps. The multirobot_map_merge node that I used was originally written for gmapping and produced maps with the same size and origin for all robots. However, I decided to utilize slam_toolbox instead of gmapping. This meant that I had to expand the maps I recieved from slam_toolbox and resize them so that all robot maps had matching origins and sizes. 

This wasn't as easy as just redefining the map's width, height and origin; I also had to edit the map's data. The new map's width * height had to match the size of the cell data and if I simply redefined the map's dimensions without actually adding any more cell data, I would have encountd a memory deallocation error in C++. 

Therefore I wrote my own map_expansion node to solve this issue. This node created a new map with pixel size 384 x 384 and an origin at (-10, -10). I then read in the slam_toolbox map data and, given the difference in size and origin between the slam_toolbox map and new map, I was able to fill in the remaining space with unknown cells to explore. 

Please see the image below to visualize the sections of the new map that were added. The black box represents the map created from slam_toolbox. The map expansion node fills in the yellow box, then the orange boxes and finally the green box with unknown cells for the new map. 

<p align="center">
  <img width="600" height="200" src="{{ site.baseurl }}/images/map_expansion.png">
</p>

## Map Merging
Once all the maps were of the appropirate size and the robots we're able to explore frontiers, it was time to start merging the maps. Please see the flowchart below to understand the structure of merging multiple robot maps. 

<p align="center">
  <img width="800" height="600" src="{{ site.baseurl }}/images/map_flowchart.png">
</p>

For operations where you know the inital positions of the robots, map merging is relative strightforward. However, in a realistic setting, its unlikely that someone would have that kind of control or knowledge. But without knowing the robot positions, how can you merge the maps? The multirobot_map_merge node uses feature matching to stitch the sperate maps into one. Unfortunately this functionality has a catch, you have to initially place the robot's very close to each other so there is enough overlap for the feature matching algorithm to work. 

In my continued work on this project, I'll be tackling this issue and develop a method where the robots can start exploring without any information about the other while a feature detection algothrim works in the background to detected matching areas of the maps. Once common ground is found, I will then initiate the map merging process and reconfigure the robot's to explore frontiers on the combined map instead of their local maps. 

## Future Development
Aside from the development I mentioned above I am also working on making this software more modular so that adding more robots to this operation is as simple as adding another namespace. I'll soon be running frontier exploration and map mergering with my currnet software on two actual Turtlebots as well. 