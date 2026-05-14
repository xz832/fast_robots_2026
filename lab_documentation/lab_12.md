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

3. Since 
since from lab 11 my localization is pretty poor, choose not to trust car, localize at every waypoint
calculate angle to next way point --> execute PID turn control
read distance --> calculate distance to next waypoint --> execute PID linear control
repeat

Arduino:
modify lab 11 code for only going to one target angle --> might have to change my initialization
modify lab 5 code for going to set distance



capture both live video and mapped path