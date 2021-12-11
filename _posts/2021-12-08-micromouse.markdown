---
layout: post
title:  Micromouse Predator from Scratch
date:   2021-12-08 12:00:00 +0300
image:  mm/robot.gif
tags:   Mechatronics, C, PCB Design, Solidworks
---

# Project Overview

The goal of this project was to design, construct, and control a small wheeled robot, completely from scratch. Dr. Malcolm MacIver’s lab is studying robot-rodent interactions and required a robot to act as a high-speed predator that will constantly try to discover, chase, and corner a mouse in a maze-like habitat. 

To achieve this, I drew knowledge from my previous mechatronics experience to build a motor control circuit, communicate wirelessly using a Digi XBee module radio, as well as create a custom PCB. I also took inspiration from the <a href="https://en.wikipedia.org/wiki/Micromouse" target="_blank" rel="noopener noreferrer">Half-Sized Micromouse Competition</a> for both mechanical and electrical design. 

Have a look at the code on my <a href="https://github.com/gingineer95/Micromouse_Predator" target="_blank" rel="noopener noreferrer">GitHub</a>.

## Design Constraints and Considerations

Overall, this project was relatively open-ended. The three hard constraints were that the robot (1) be no wider than 6.35cm in order to fit through the smallest opening in the habitat, (2) move as fast, if not faster than the mice, which peak at speeds around 2m/s, and (3) make 60 degree turns around 1 m/s. 

While there were no height or weight restrictions for this robot, those variables certainly played a significant role in my design process. If the center of mass of the robot was too high, the robot would tip over when making high-speed turns. And the heavier the total mass of the robot became, the larger the motors would have to become to create the appropriate driving torque. Given these considerations coupled with the fact that speed and width we’re the main constraints on this robot, I first turned my focus to analyzing and comparing different motors.

![forward_back](https://user-images.githubusercontent.com/70979347/145663630-b14f3423-a97c-4694-a2b0-1232f6001867.gif)

## Motor Analysis

My goal was to create a simulated “race” between a given brushed DC motor and the mice to find motors that ran faster than the mice. Dr. MaIver’s lab provided me with mouse trajectory and velocity data over time from their ongoing experiments. With this information to compare to, I needed to extrapolate equations to calculate a motor’s position and linear velocity over time given only a motor’s electrical specs. 

To model the motor’s behavior, I started using a simple equation that expressed input and outputs in terms of power. I manipulated that equation until I had an expression that was only dependent on the 1st and 2nd order differentials of displacement over time. Once I had a function that was dependent on displacement over time, I was able to run the “race” simualtions. In order to get the robots linear and velocity displacement over time to compare to they mice, solved the equation for *x(t)* as well as *x'(t)*:

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\large&space;{\color{White}&space;x(t)&space;=&space;\frac{rV(k_t^2G^2&space;t&space;&plus;&space;Rm{r}^2&space;({e}^{-\frac{k_t^2G^2&space;t}{Rm{r}^2}}&space;-&space;1))}{k_t^3G^3}}" title="\large {\color{White} x(t) = \frac{rV(k_t^2G^2 t + Rm{r}^2 ({e}^{-\frac{k_t^2G^2 t}{Rm{r}^2}} - 1))}{k_t^3G^3}}" />
</p>
<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\large&space;{\color{White}&space;x'(t)&space;=&space;\frac{rV}{k_tG}&space;(1&space;-&space;{e}^{-\frac{k_t^2G^2&space;t}{Rm{r}^2}})}" title="\large {\color{White} x'(t) = \frac{rV}{k_tG} (1 - {e}^{-\frac{k_t^2G^2 t}{Rm{r}^2}})}" />
</p>


Where *r* is the wheel radius, *V* is the voltage across the motor, *kt* is the torque constant, *G* is the gear ratio, *R* is the motor's internal resistance and *m* is the total mass of the robot and all its parts. Using the equations, I compared several motors to the mice by creating graphs such as the two below. The blue line represents the motor’s data while the green line represents a mouse.

<p align="center">
  <img width="2000" height="1000" src="{{ site.baseurl }}/images/mm/mouse_race_data.png">
</p>

I also examined each motor’s torque-speed curve to ensure that a motor’s anticipated torque outputs were within the continuous operating region of that motor. Using all graphs I was able to eliminate several motors while still having a couple of viable motors to choose from. 

My final motor selection came down to weight and size. For this entire system, the motors and battery we’re the largest and heaviest components. Therefore I wanted the smallest and lightest option of both. This meant choosing a 1 cell battery and therefore a motor that only needed up to 3.7V to operate. After doing some initial mechanical modeling, I chose a light and small motor that fit the design configuration I was looking for. 

## Circuit Hardware

Once the critical parts were chosen, it was time to order all the other circuit components so I could start breadboarding and testing the circuit before ordering a custom PCB. I chose to use a PIC32 microcontroller since I had previous experience with these chips and there were a couple on hand to get started with quickly. Along those lines, I also chose to use the TI DRV8833 motor drivers. As a safety precaution, I added IR distance sensors to detect objects and avoid collisions. The last key component to this circuit was the Digi XBee 3 802.15.4 module which I used to communicate wirelessly with the robot. 

Given these components, I created the following schematic and tested this circuit on the following breadboard:

<p align="center">
  <img width="2000" height="2000" src="{{ site.baseurl }}/images/mm/sch1.png">
  <img width="2000" height="2000" src="{{ site.baseurl }}/images/mm/sch2.png">
  <img width="2000" height="2000" src="{{ site.baseurl }}/images/mm/sch3.png">
  <img width="2000" height="2000" src="{{ site.baseurl }}/images/mm/breadboard.jpg">
</p>

## PCB Design

In order to integrate all the circuit components into a small area on the robot, a custom PCB was necessary. After I confirmed that my circuit worked on the breadboard, I had confidence that I could design a functional PCB. 

My initial thought was to use the PCB as a chassis component, as many of the Micromouse robots do. However, the MacIver lab uses a camera system above their habitat to track the mice and robot. And in order to detect, track, and determine the orientation of the robot, 3 red LEDs need to form an isosceles triangle on top of the robot. Given that, it was simple to rework the mechanical design so the PCB was the top-most component.

As with all PCB prototyping, this was a very iterative process, especially considering how many components needed to fit in such a small space. The dimensions of the PCB were determined by the footprint of the robot, which in turn was driven by the length of the motors. I was able to fit 35 components in a 46cm x 34cm space as pictured below.

<div align="center">View from OSHPark</div>
<p align="center">
  <img width="600" height="600" src="{{ site.baseurl }}/images/mm/osh_park.png">
</p>

Once the boards were manufactured, I soldered all the individual components myself using a microscope. I quickly realized I couldn’t program my microcontroller. I did a continuity test to identify the issue and discovered I didn’t have common ground across all the components. To get my PCB up and running as quickly as possible, I soldered jumper wires to all components that needed common ground. After that I was able to program the PIC and confirm that all my other connections were correct. I designed a new board with common ground for future use. 

<div align="center">Left side shows completed board with the XBee module.</div>
<div align="center">Right side shows the components lying underneath the XBee.</div>
<p align="center">
  <img width="350" height="300" src="{{ site.baseurl }}/images/mm/board_wXB.jpg">
  <img width="350" height="300" src="{{ site.baseurl }}/images/mm/board_noXB.jpg">
</p>

## Mechanical Design

Below are images of the finalized, differential drive robot that I modeled in Soldiworks and then 3D printed.

<div align="center">Final design, 5cm wide by 5.5cm long</div>
<p align="center">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/side_views/side1.jpg">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/side_views/side2.jpg">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/side_views/side3.jpg">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/iso_views/iso1.jpg">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/iso_views/iso3.jpg">
  <img width="250" height="250" src="{{ site.baseurl }}/images/mm/iso_views/iso5.jpg">
</p>

I started with a four wheel configuration like many of the Micromouse designs. But after some testing, I discovered that the flooring in the MacIver lab habitat has a much higher coefficient of friction than the flooring used in the Micromouse competitions. I decided to remove two wheels and switch the tire material to a harder rubber o-ring which were skinner and therefore had smaller areas of contact to make turns easier. The thinner wheels also had the benefit of making the overall width of the robot smaller.

<div align="center">Orignal design on left, updated design on right</div>
<p align="center">
  <img width="350" height="300" src="{{ site.baseurl }}/images/mm/4wheel.jpg">
  <img width="350" height="300" src="{{ site.baseurl }}/images/mm/2wheel.jpg">
</p>

## Remote Control

Below is a video of robot being given left and right DC commands. It shows the robot moving forward and backwards, turning in a circle, and lastly moving in a loop. Note: these are only feed-forward commands, feed-back control is needed for precise movement.

<iframe width="560" height="315" src="https://www.youtube.com/embed/UWJekWSlxOU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Next Steps

As a safety feature the next immediate step will be incorporating the IR distance sensors as object avoidance / collision detection. I have already accounted for the sensors in my PCB, so all there is left to do is mount the hardware and implement functionality onto the microchip such that the robot stops if it senses that an object is too close. 

Given that there is a mechanically and electrically sound design, the next control steps are to add a PID controller. The MacIver lab is able to track the robot's location as it moves throughout the habitat. With that information, a position feedback controller can be implemented per the block diagram below. 

<p align="center">
  <img width="800" height="800" src="{{ site.baseurl }}/images/mm/pid_block.png">
</p>

Once the PID controller is squared away, the final step for autonomous navigation will be to create a path planning algotihm. This algorithm will recevice a mouse location and plan a route from the robot's location to the mouse while avoiding all obstacles. With that, the predator robot will be fully operational!