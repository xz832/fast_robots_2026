---
layout: default
title: "Lab 5"
permalink: /lab_documentation/lab_5/
description: "writeup for lab 5"
---


# LAB 5 - MAE4190 FAST ROBOTS

Welcome to lab 5 of fast robots! In this lab we will be implementing linear PID control on our car.

## Prelab

Start PID controls

```C++
case START_CONTROL:

    success = robot_cmd.get_next_value(Kp);
    if (!success)
        return;

    success = robot_cmd.get_next_value(Ki);
    if (!success)
        return;

    success = robot_cmd.get_next_value(Kd);
    if (!success)
        return;

    success = robot_cmd.get_next_value(target_dis);
    if (!success)
        return;

    start_PID = true;

    break;
```

Stop PID controls

```C++
    case STOP_CONTROL:

        start_PID = false;
        control_stop();

        Serial.println("PID stopped");
            
        break;
```

Send PID controls
(arduino & python; show stored arrays?)

Aside from the stopping protocol 
just describe hardstop (sets all PWM to 0 once BLE disconnects)

adjustment for deadband, adjustment for wind-up protection

**
Clearly describe how you handle sending and receiving data over Bluetooth
Consider adding code snippets as necessary to showcase how you implemented this on Arduino and Python

Have the robot controller start on an input from your computer sent over Bluetooth
Execute PID control over a fixed amount of time (e.g. 5s) while storing debugging data in arrays.
Remember to have a hard stop implemented directly on your Artemis, so that your robot will stop even if the Bluetooth connection fails.
Upon completion of the behavior, send the debugging data back to the computer over Bluetooth.

## Lab Procedure

### Position Control

![PID_equation](../images/Lab5/PID_eq.png)

Kp control:
starting out with only proportional control, starting Kp at around 0.05 to try with a very mild controller

![P_0.5](../images/Lab5/P_0.5.png)

0.04:
There is not much point in placing the Kp value lower than 0.05 as it is already a very gentle controller, and after trying 0.04 and getting the values of the motor inputs, most of it is capped at the lower limit of 40. I tried a more aggressive Kp instead.

0.09:
This is about the largest I can make Kp without the car hitting the wall. As we can see the overshoot is much larger and it takes a lot longer to recover and oscillate around the desired location.

![P_0.09](../images/Lab5/P_0.09.png)

Since the 0.05 controller is about right for accuracy, but a little weak for my carpeted floor, I chose Kp = 0.06.

Kd control:

trying to reduce oscillation --> starting with 0.1, more aggressive; oops too aggressive, my car ran into the wall
down to 0.05: better, still running into the wall
settled at 0.01, enough to reduce oscillations, but not enough to fully ram into wall. Able to recover

unfortunately at this point my tof sensor broke and when .getDistance() was run returns one constant distance. I had to switch to my other sensor. Hope this one doesn't break!

controlling overshoot with derivative

Ki control:

need anti wind up --> small Ki, overshooting a lot because it's cumulative


why PID?

Control more accurately, reduce oscillation/overshoot while increasing power??

### Extrapolation

**
P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
Range/Sampling time discussion
Graphs, code, videos, images, discussion of reaching task goal
Graph data should include Tof vs time and Motor input vs time (and whatever helps with debugging)


## References

I took reference 


**missing:
testing, troubleshooting,
extrapolation stuff
running PID loop without TOF data