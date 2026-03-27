---
layout: default
title: "Lab76"
permalink: /lab_documentation/lab_7/
description: "writeup for lab 7"
---


# LAB 7 - MAE4190 FAST ROBOTS

Welcome to lab 7 of fast robots! In this lab we will be implementing the Kalman filter on our car controls.

## Lab Tasks

In order to implement a Kalman Filter, we will first be needing a state space model of the car.

![state_space_eq](../images/Lab7/state_space_qe.png)

To get the estimates for our A and B matrices, we will therefore need the car's drag and momentum.

#### Testing for Drag and Momentum

Using a step response and giving the motors a constant input, I obtained ToF sensor data with which I calculated velocity to plot against time. From the plots I obtained the steady state velocity and 90% rise time for the d and m calculations:

![d_eq](../images/Lab7/d_calc.png)
![m_eq](../images/Lab7/m_calc.png)
