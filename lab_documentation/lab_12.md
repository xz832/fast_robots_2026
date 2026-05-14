---
layout: default
title: "Lab 12"
permalink: /lab_documentation/lab_12/
description: "writeup for lab 12"
---


# LAB 12 - MAE4190 FAST ROBOTS

This is lab 12 of fast robots. In this lab, the task is open ended, so I have decided to try and attempt a local path planning procedure through the map that we had been given.

### Attempt at Feedback Control

I started out trying to use the positional PID controls with the TOF sensors. This was my workflow:
1. At each waypoint, a python function would calculate the angle towards the next waypoint, and an arduino command, upon receiving the required angle, would run PID orientation control to steer the car towards it:

```python

```C++

```

The arduino function is modified from the localization code, but with one set target angle instead of a gradually incrementing one.

2. Another python function will calculated the target distance that the car needs to travel towards the next waypoint, with which another arduino command would execute PID linear motion control to arrive at the next waypoint:

```python
TOF reading too!

```C++
```

The arduino function is modified from 

3. Since as seen from lab 11, the control of my car is fairly poor, so I chose not to trust calculated expected control inputs, and decided to localize at every waypoint

This did unfortunately make the entire path planning process incredibly slow, as it takes a long time for the car to localize as well as send data over through the BLE.


The TOF feedback controls proved difficult as the angular adjustments were not the most reliable