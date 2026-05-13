---
layout: default
title: "Lab 12"
permalink: /lab_documentation/lab_12/
description: "writeup for lab 12"
---


# LAB 12 - MAE4190 FAST ROBOTS

This is lab 12 of fast robots. In this lab, the task is open ended, so I have decided to try and attempt a local path planning procedure through the map that we had been given.

I have python for bayes --> should I localize at every waypoint?

At each waypoint: calculate target angle to next waypoint, calculate target distance to next waypoint


1. (-4, -3)    <--start
2. (-2, -1)
3. (1, -1)
4. (2, -3)
5. (5, -3)
6. (5, -2)
7. (5, 3)
8. (0, 3)
9. (0, 0)      <--end

array of all waypoints --> cycle through
since from lab 11 my localization is pretty poor, choose not to trust car, localize at every waypoint
calculate angle to next way point --> execute PID turn control
read distance --> calculate distance to next waypoint --> execute PID linear control
repeat

Arduino:
modify lab 11 code for only going to one target distance
modify lab 5 code for going to set distance



capture both live video and mapped path