---
layout: default
title: "Lab 9"
permalink: /lab_documentation/lab_9/
description: "writeup for lab 9"
---


# LAB 9 - MAE4190 FAST ROBOTS

This is lab 9 of fast robots. In this lab, our objective is to map out a static room. To build the map, we will place your robot in a couple of marked locations around the lab, and have it spin around its axis while collecting ToF readings.

### Control

I chose to perform this task with a PID orientation control.

I added tape to my wheels as from previous labs it is known that my car has trouble turning and requires a lot of motor power to start the turn. For such a slow speed, precision based turn, I'll need it to be a smoother motion and ideally not giving the motors a burst of power and sending the car to overshoot.

I want the car to be abel to turn 360 regardless of starting orientation


***TO DO:

LAB 9 MAKE CAR ABEL TO TURN 360 REGARDLESS OF STARTING ORIENTATION
debugging:
speed adjustment
angles not matching up due to wrapping/lack thereof
NAN outputs

angle wrapping/PID recognition --> sometimes it thinks it's 180 away (30 to -150 etc)

ble.send_command(CMD.START_SPIN, "1|0.3|0.0001") --> originally 2.2
#Kp|Kd|Ki

motor speed adjusting

the point turn graphs look like that because the spin was not long enough to fill the full array sent over bluetooth


1. Derivative control spiking in the start (low pass filter with raw gyro data + time step being too small no guard for that)
2. starting angles change to offset