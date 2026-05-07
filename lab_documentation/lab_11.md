---
layout: default
title: "Lab 11"
permalink: /lab_documentation/lab_11/
description: "writeup for lab 11"
---


# LAB 11 - MAE4190 FAST ROBOTS

This is lab 11 of fast robots. In this lab, our objective is perform localization with the Bayes filter on our actual robot, and observe the differences between simulation from lab 10 and a real world system.

### Lab Tasks

In order to match the observation loop in world.yaml of our base code and to ensure the accuracy of my localized pose, I made adjustments to my lab 9 code. I decreased the increment at which my robot rotates to take a tof sensor reading to 20, and accordingly increased the number of turns to 18.

positive angles are in the counterclockwise direction

1. Testing Bayes filter in simulator

![lab_11_sim](../images/Lab11/lab_11_sim.png)

Testing the localization in the sim, the plot shows odometry in red, ground truth in green and belief in blue. The odometry is visibly quite off from what the path should be, due to the error that accumulates with each iteration. The ground truth and the belief follow each other closely, showing that the Bayes filter working well in simulation.

labs only opens on Thursday!!!