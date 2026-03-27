---
layout: default
title: "Lab 6"
permalink: /lab_documentation/lab_6/
description: "writeup for lab 6"
---


# LAB 6 - MAE4190 FAST ROBOTS

Welcome to lab 6 of fast robots! In this lab we will be implementing orientation PID control on our car. This lab will involve controlling the yaw of your robot using the IMU.

## Prelab

Similar to lab 5, but changed some variables

Clearly describe how you handle sending and receiving data over Bluetooth
Consider adding code snippets as necessary to showcase how you implemented this on Arduino and Python


## Lab Tasks


***
Are there limitations on the sensor itself to be aware of? What is the maximum rotational velocity that the gyroscope can read (look at spec sheets and code documentation on github). Is this sufficient for our applications, and is there a way to configure this parameter?

the DMP is capable of error and drift correction by fusing readings from the ICM’s 3-axis gyroscope, 3-axis accelerometer, 3-axis magnetometer/compass.

According to the the ICM-20948 gyroscope has a maximum rotational velocity range of ±250/±500/±1000/±2000 DPS (degrees per second) for 3-axis gyroscope. The ±250 is typcially the default of ICM-20948, which is too small for rapid rotations. However, with the DMP automatically uses ±2000 DPS, and this is sufficient for the purposes of this lab.
***

DMP implementation needed --> I also decided to include the visualization as it may help with the debugging process

P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
Range/Sampling time discussion
Graphs, code, videos, images, discussion of reaching task goal
Graph data should at least include theta vs time (you can also consider angular velocity, motor input, etc)