---
layout: default
title: "Lab 4"
permalink: /lab_documentation/lab_4/
description: "writeup for lab 4"
---


# LAB 4 - MAE4190 FAST ROBOTS

Welcome to lab 4 of fast robots! In this lab we change from manual to open loop control of the car with the Artemis and two dual motor drivers.

## Prelab

![wiring](../images/Lab4/wiring.png)
Diagram with your intended connections between the motor drivers, Artemis, and battery (with specific pin numbers)
Battery discussion

In your lab write-up, discuss/show how you decide to hook up/place the motor drivers.
analog pins
We ask you to power the Artemis and the motor drivers/motors from separate batteries. Why is that?
length of wires
the battery must be detachable for charging --> solder to connectors

```C++
void loop() {
  Serial.println("DRIVE");
  analogWrite(MOTOR1PIN1, speed);
  //analogWrite(MOTOR2PIN1, speed);
  delay(3000);

  analogWrite(MOTOR1PIN1, 0);
  //analogWrite(MOTOR2PIN1, 0);
  delay(3000);
}
```


Lab Tasks
Picture of your setup with power supply and oscilloscope hookup
Power supply setting discussion
Include the code snippet for your analogWrite code that tests the motor drivers
Image of your oscilloscope
Short video of wheels spinning as expected (including code snippet it’s running on)
Short video of both wheels spinning (with battery driving the motor drivers)
Picture of all the components secured in the car
Consider labeling your picture if you can’t see all the components
Lower limit PWM value discussion
Calibration demonstration (discussion, video, code, pictures as needed)
Open loop code and video