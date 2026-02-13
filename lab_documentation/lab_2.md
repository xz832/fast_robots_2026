---
layout: default
title: "Lab 1"
permalink: /lab_documentation/lab_2/
description: "writeup for lab 2"
---


# LAB 2 - MAE4190 FAST ROBOTS

## Lab Tasks

### Set up the IMU
Picture of your Artemis IMU connections
Show that the IMU example code works
![imu_connect](../images/Lab2/imu_connect.jpg)
![example_code](../images/Lab2/example.gif)

The IMU offers two I2C addresses, corresponding to the two different pins. Two different AD0 configurations are needed to distinguish between the higher and lower addresses while connected to the same I2C bus. That is the purpose of the AD0_VAL. The default is 1, and when the ADR jumper is closed the value becomes 0.

Acceleration and gyroscope data discussion (pictures recommended)


### Accelerometer
Image of output at {-90, 0, 90} degrees for pitch and roll (include equations)
Accelerometer accuracy discussion
Noise in the frequency spectrum analysis
Include graphs for your fourier transform
Discuss the results
### Gyroscope
Include documentation for pitch, roll, and yaw with images of the results of different IMU positions
Demonstrate the accuracy and range of the complementary filter, and discuss any design choices
### Sample Data
Speed of sampling discussion
Demonstrate collected and stored time-stamped IMU data in arrays
Demonstrate 5s of IMU data sent over Bluetooth
### Record a Stunt
Include a video (or some videos) of you playing with the car and discuss your observations