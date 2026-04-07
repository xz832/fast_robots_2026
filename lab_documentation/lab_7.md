---
layout: default
title: "Lab7"
permalink: /lab_documentation/lab_7/
description: "writeup for lab 7"
---


# LAB 7 - MAE4190 FAST ROBOTS

Welcome to lab 7 of fast robots! In this lab we will be implementing the Kalman filter on our car controls.

## Lab Tasks

In order to implement a Kalman Filter, we will first be needing a state space model of the car.

![state_space_eq](../images/Lab7/state_space_qe.png)

To get the estimates for our A and B matrices, we will therefore need the car's drag and momentum.

#### 1. Testing for Drag and Momentum

Using a step response and giving the motors a constant input, I obtained ToF sensor data with which I calculated velocity to plot against time. From the plots I obtained the steady state velocity and 90% rise time for the d and m calculations:

![d_eq](../images/Lab7/d_calc.png)
![m_eq](../images/Lab7/m_calc.png)

I chose the step response input to be _____
placed car ____ distance from wall
active braking


```C+++
            while (time_interval < step_time) {
                time_interval = millis() - start_time;

                //tof sensor
                if (distanceSensor2.checkForDataReady()){
                    drag_dis = distanceSensor2.getDistance();
                    distanceSensor2.clearInterrupt();
                    distance_doc[tindex] = drag_dis;
                    time_doc[tindex] = millis() - start_time;
                    tindex++;
                }

                //active braking
                if (drag_dis < target_dis) {
                    control_stop();
                    return;
                }

                //drive forwards
                analogWrite(MOTOR1PIN1, step_speed);
                analogWrite(MOTOR2PIN1, step_speed * 1.4);
                analogWrite(MOTOR1PIN2, 0);
                analogWrite(MOTOR2PIN2, 0);

            }

            control_stop();
            delay(1000);
            distanceSensor2.stopRanging();
```

Decided to save my data with a csv file

velocity = dd / dt
velocity_KF = dd_KF / dt
dt = dt[mask]
dd = dd[mask]
dd = np.diff(distances)
dd_KF = np.diff(distances_KF)
dt = np.diff(times)

velocity_values = []
for i in range(len(tof2_time_values) - 1):  # Loop over indices
    if (tof2_time_values[i] == tof2_time_values[0]):
        dt = 1
        dd = .000001
    else:
        dt = tof2_time_values[i] - tof2_time_values[i-1]  # Time difference
        dd = tof2_values[i] - tof2_values[i-1]  # Distance difference
    
    velocity = dd / dt  # Velocity calculation
    velocity_values.append(round(velocity,3))

velocity_values.append(0) # ensure arrays of equal length



11am-3pm class + shift, REMEMBER TO GO GET POSTERS
3-5pm coding
5-7pm rover (encoders?)
Finish lab 7 today or I will explode

to be of similar size to the PWM value you used in Lab 5 (to keep the dynamics similar). Pick something between 50%-100% of the maximum u.


unfortunately at this point one of my motor drivers broke, so I will submit the videos and testing later during break on someone else's car
