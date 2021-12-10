---
layout: post
title:  Micromouse
date:   2021-12-11 12:00:00 +0300
image:  FANUC.gif
tags:   Mechatronics, C, PCB Design, Solidworks
---

# Project Overview:

The goal of this project was to design, construct, and control a small wheeled robot, completely from scratch. Dr. Malcolm MacIver’s lab is studying robot-rodent interactions and required a robot to act as a high-speed predator that will constantly try to discover, chase, and corner a mouse in a maze-like habitat. 

To achieve this, I drew knowledge from my previous mechatronics experience to build a motor control circuit, communicate wirelessly using a Digi XBee module radio, as well as create a custom PCB. I also took inspiration from the Half-Sized Micromouse Competition for both mechanical and electrical design. 

CHECK OUT MY GITHUB.

## Design Constraints and Considerations

Overall, this project was relatively open-ended. The three hard constraints were that the robot (1) be no wider than 6.35cm in order to fit through the smallest opening in the habitat, (2) move as fast, if not faster than the mice, which peak at speeds around 2m/s, and (3) make 60 degree turns around 1 m/s. 

While there were no height or weight restrictions for this robot, those variables certainly played a significant role in my design process. If the center of mass of the robot was too high, the robot would tip over when making high-speed turns. And the heavier the total mass of the robot became, the larger the motors would have to become to create the appropriate driving torque. Given these considerations coupled with the fact that speed and width we’re the main constraints on this robot, I first turned my focus to analyzing and comparing different motors. 

## Motor Analysis

My goal was to create a simulated “race” between a given brushed DC motor and the mice to find motors that ran faster than the mice. Dr. MaIver’s lab provided me with mouse trajectory and velocity data over time from their ongoing experiments. With this information to compare to, I needed to extrapolate equations to calculate a motor’s position and linear velocity over time given only a motor’s electrical specs. 

To model the motor’s behavior, I started using a simple equation that expresses input and outputs in terms of power below [1]:

$IV = \tau \omega + {I}^2 R$		(Equation 1)

Where $I$ is the current through the motor, $V$ is the voltage across the motor, $\tau$ and $\omega$ stand for torque and angular velocity on the output shaft and $R$ is the motor’s internal resistance. The left side of the equation accounts for the electrical input power into a motor, while the right side accounts for the mechanical output power and power expelled through heat dissipation. 

After substituting in the torque constant $k_t = \frac{\tau}{I}$, $\tau = mar$ tor torque and $\omega = \frac{v}{r}$ for omega, I solved for an equation that is represented by the 1st and 2nd and 1st order differentials of displacement over time:

$a + \frac{v {k_t}^2}{Rm{r}^2} - \frac{V k_t}{Rmr} = 0$

$\frac{{d}^2 x}{d{t}^2} + \frac{\frac{dx}{dt} {k_t}^2}{Rm{r}^2} - \frac{V k_t}{Rmr} = 0$	(Equation 2)

Once I had a function that was dependent on displacement over time, I was able to run the “race” simualtions. In order to get the robots linear and velocity displacement over time to compare to they mice, solved the equation for $x(t)$ as well as $x'(t)$:

$x(t) = \frac{rV( {(k_t G)}^2 t + Rm{r}^2 ({e}^{-\frac{{(k_t G)}^2 t}{Rm{r}^2}} - 1))}{{(k_t G)}^3}$	(Equation 3)

$x'(t) = \frac{rV}{(k_t G)} (1 - {e}^{-\frac{{(k_t G)}^2 t}{Rm{r}^2}})$ 	(Equation 4)

Using the equations, I compared several motors to the mice by creating graphs such as the two below. The blue line represents the motor’s data while the green line represents a mouse:


I also examined each motor’s torque-speed curve to ensure that a motor’s anticipated torque outputs were within the continuous operating region of that motor. Using all graphs I was able to eliminate several motors while still having a couple of viable motors to choose from. 

My final motor selection came down to weight and size. For this entire system, the motors and battery we’re the largest and heaviest components. Therefore I wanted the smallest and lightest option of both. This meant choosing a 1 cell battery and therefore a motor that only needed up to 3.7V to operate. After doing some initial mechanical modeling, I chose a light and small motor that fit the design configuration I was looking for. 

## Circuit Hardware

Once the motor and batteries were chosen, it was time to order all the other circuit components so I could start breadboarding and testing the circuit before ordering a custom PCB. I chose to use a PIC32 microcontroller since I had previous experience with these chips and I had a couple on hand to get started with quickly. Along those lines, I also chose to use the TI DRV8833 motor drivers. As a safety precaution to not hit mice, I added IR distance sensors. The last key component was the Digi XBee 3 802.15.4 module which I used to communicate wirelessly to and from the robot. 

Given these and a couple of other ancillary components, here is a schematic:



Breadboard with motors working:



Robot running off breadboard:



(talk about LEDs making an isosceles triangle to locate and track robot)

## PCB Design

In order to integrate all the circuit components into a small area, a custom PCB was necessary. After I confirmed that my circuit worked on the breadboard, I had confidence that I could design a functional PCB. 

My initial thought was to use the PCB as a chassis component, as many of the micro mouse robots do. However, the MacIver lab uses a camera system above the habitat to track the mice and robot. And in order to detect, track, and determine the orientation of the robot, 3 red LEDs need to form an isosceles triangle on top of the robot. Given that, it was simple to rework the mechanical design so the PCB was the top-most component.

As with all PCB prototyping, this was a very iterative process, especially considering how many components needed to fit in such a small space. Small space was determined by the footprint of the robot, which was determined by the length of the motors. Fit 30-something components in a 50cm by 30cm space. See images below:

(Insert PCB design)

After reworking my original board design a couple of times, I finally sent it out for manufacturing. 
When the boards arrived, I soldered everything up myself (using a microscope). When I went to start programming my PIC, I couldn’t properly connect vis my SNAP. To debug, I did a continuity test and discovered I didn’t have common ground across all the components. To get things up and running as quickly as possible, I soldered jumper wires to all components that needed common ground. Once I did that, programming the PIC and using all the other components went off without a hitch. I designed a new board with common ground for future use. 

(show actual current board).


## Iterative Mechanical Design
...
...
... 

## Future Development
