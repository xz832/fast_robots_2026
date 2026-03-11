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

just describe hardstop (sets all PWM to 0 once BLE disconnects)

**
Clearly describe how you handle sending and receiving data over Bluetooth
Consider adding code snippets as necessary to showcase how you implemented this on Arduino and Python

Have the robot controller start on an input from your computer sent over Bluetooth
Execute PID control over a fixed amount of time (e.g. 5s) while storing debugging data in arrays.
Remember to have a hard stop implemented directly on your Artemis, so that your robot will stop even if the Bluetooth connection fails.
Upon completion of the behavior, send the debugging data back to the computer over Bluetooth.


## References

I took reference 