---
layout: default
title: "Lab 1"
permalink: /lab_documentation/lab_2/
description: "writeup for lab 2"
---


# LAB 2 - MAE4190 FAST ROBOTS

## Lab Tasks

### Set up the IMU
![imu_connect](../images/Lab2/imu_connect.jpg)
![example_code](../images/Lab2/example.gif)
You can see the numbers changing as I change the orientation of the IMU.
The acceleration values change according to the orientation of the IMU, while the gyroscope data changes based on the motion of me moving the IMU (angular velocity).

The IMU offers two I2C addresses, corresponding to the two different pins. Two different AD0 configurations are needed to distinguish between the higher and lower addresses while connected to the same I2C bus. That is the purpose of the AD0_VAL. The default is 1, and when the ADR jumper is closed the value becomes 0.

### Accelerometer

I converted the data from the accelerometer to roll and pitch using the equations from lecture:
```C++
case ACC_PITCHROLL: {

          for (int tindex = 0; tindex < 3000; tindex++){
            time_doc[tindex] = millis();
            
            myICM.getAGMT();
            float ax = myICM.accX();
            float ay = myICM.accY();
            float az = myICM.accZ();

            float roll = atan2(ay, az) * 180/M_PI;
            float pitch = atan2(ax, az) * 180/M_PI;

```

This is the output of the pitch and roll when the IMU is at {-90, 0, 90}.
![output_90](../images/Lab2/pitchroll.png)

The roll of the accelerometer is fairly accurate, I set it squarely against the table and the reading is consistently around -90 degrees. Small adjustments are also pretty stable. However, the pitch fluctuates a lot, potentially due to the vibrations along my table. It also seems to have a slight offset, outputting values up to 166 degrees when it should be at 90.

Here is the jupyter graph that generates with the data obtained with an arbitrary orientation.
![acc_graph](../images/Lab2/acc_data_graph.png)


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

(Side note to self) I accidentally uploaded some files that were way too big and the way to fix that is to get the log of commits and dial back to the commit before the large file