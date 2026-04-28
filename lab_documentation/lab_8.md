---
layout: default
title: "Lab8"
permalink: /lab_documentation/lab_8/
description: "writeup for lab 8"
---


# LAB 8 - MAE4190 FAST ROBOTS

Welcome to lab 8 of fast robots! In this lab we will be doing stunts with our cars! I chose to do the flip.

In order to control the car, I implemented the following code, fairly similar to that of the previous lab 5 and 6:

```C++
        case START_STUNT:

            start_STUNT = true;

            break;
        /*
         * Allowing the car to be stopped via BLE
         */
        case STOP_STUNT:

            start_STUNT = false;
            control_stop();

            Serial.println("Stunt stopped");
            
            break;
        
        case SEND_STUNT_DATA:

            for (int tindex = 0; tindex < tindex_max; tindex++){
                
                tx_estring_value.clear();
                //send time data
                tx_estring_value.append((float)time_doc[tindex]);
                tx_estring_value.append(",");
                //send distance data
                tx_estring_value.append((float)distance_doc[tindex]);
                tx_estring_value.append(",");
                //send motor input
                tx_estring_value.append((float)motor_input[tindex]);

                tx_characteristic_string.writeValue(tx_estring_value.c_str());
                delay(1000);

            }

            break;
```

## Hard Coded Flip

To test if my hardware is able to do a flip first, I skipped the implementation of flipping based on my car's distance from the wall using my TOF sensors. Instead, I hard-coded the flip just by running the car at full speed for a set amount of time, stopping it briefly, and driving the other direction afterwards. The code is as follows:

```C++
//stunt control
            if (start_STUNT == true && tindex < tindex_max) {
                unsigned long timer = millis();
                while ((millis()-timer < 900)){
                    //if (distanceSensor2.checkForDataReady()){
                        //float distance1 = distanceSensor2.getDistance(); //Get the result of the measurement from the sensor
                        //distance_doc[tindex] = distance1;
                        //Serial.print(distance1);
                        //distanceSensor2.clearInterrupt();

                        analogWrite(MOTOR1PIN1, 240);
                        analogWrite(MOTOR2PIN1, 240);
                        analogWrite(MOTOR1PIN2, 0);
                        analogWrite(MOTOR2PIN2, 0);

                        time_doc[tindex] = millis();

                        tindex++;
                    //}
                }
                analogWrite(MOTOR1PIN1, 1);
                analogWrite(MOTOR2PIN1, 1);
                analogWrite(MOTOR1PIN2, 0);
                analogWrite(MOTOR2PIN2, 0);
                //delay(10);

                control_stop();

                analogWrite(MOTOR1PIN1, 0);
                analogWrite(MOTOR2PIN1, 0);
                analogWrite(MOTOR1PIN2, 250);
                analogWrite(MOTOR2PIN2, 250);
                delay(2000);

                analogWrite(MOTOR1PIN1, 0);
                analogWrite(MOTOR2PIN1, 0);
                analogWrite(MOTOR1PIN2, 0);
                analogWrite(MOTOR2PIN2, 0);
                delay(3000);

            }
```

Interestingly at almost maximum PWM, the two motors run pretty similarly. I originally had a 1.4 ratio between their speeds, similar to other labs, but it veered off to the side. Removing it gave the car a fairly straight path. However, I did resolder one of the motor input wires between this lab and the previous few, since the wire broke during lab 7, so the speed difference could have been due to bad connection as well.

### Hardware Adjustments

The center of mass of my car is much closer towards the back, hence originally, no matter how fast I was going, the car would not flip, only going back and forth:

![wrong_com_dist](../images/Lab8/stunt_wrong_com_dist.png)
![wrong_com_motor](../images/Lab8/stunt_wrong_com_motor.png)

I added two bearings to the top of the car towards the front to shift the center of mass, as well as taped the back wheels for less friction.

![car_setup_stunt](../images/Lab8/car_setup_stunt.jpeg)

This proved successful for my car to perform the flip.

[![hard_code_flip](https://img.youtube.com/vi/vd0Yp2gF3XI/0.jpg)](https://www.youtube.com/watch?v=vd0Yp2gF3XI)


## Distance-Based Flip

Now that I have confirmed that my hardware is suited to perform the flip, I added the distance-based controls from the TOF sensor readings such that the car flips at the right distance from the wall regardless of starting position. The relevant code is as follows:

```C++
//stunt control
            if (start_STUNT == true && tindex < tindex_max) {
                if (distanceSensor2.checkForDataReady()){
                    float distance1 = distanceSensor2.getDistance(); //Get the result of the sensor
                    distance_doc[tindex] = distance1;
                    distanceSensor2.clearInterrupt();
                    time_doc[tindex] = millis();
                    if (distance1 > 805){

                        analogWrite(MOTOR1PIN1, 250);
                        analogWrite(MOTOR2PIN1, 250);
                        analogWrite(MOTOR1PIN2, 0);
                        analogWrite(MOTOR2PIN2, 0);                        

                        tindex++;

                    } else if (distance1 <= 805){
                        analogWrite(MOTOR2PIN1, 1);
                        analogWrite(MOTOR1PIN1, 1);
                        analogWrite(MOTOR1PIN2, 0);
                        analogWrite(MOTOR2PIN2, 0);

                        control_stop();

                        analogWrite(MOTOR1PIN1, 0);
                        analogWrite(MOTOR2PIN1, 0);
                        analogWrite(MOTOR1PIN2, 250);
                        analogWrite(MOTOR2PIN2, 250);
                        delay(2000);

                        analogWrite(MOTOR1PIN1, 0);
                        analogWrite(MOTOR2PIN1, 0);
                        analogWrite(MOTOR1PIN2, 0);
                        analogWrite(MOTOR2PIN2, 0);
                        delay(5000);
                    }

                }
            }
```

I started off with 305 as the benchmark for distance detection, but the TOF sensor was not fast enough to react with the flip. I incremented it in 100mm intervals and found that 805 was the distance that could reliably flip without colliding with the wall.

This was what happened for the first few attempts:

![sideways](../images/Lab8/sideways.gif)

It tends to turn sideways a little bit after flipping, possibly due to the fact that the speed at which the two wheels stop are different. One motor seems to have a delay in stopping as compared to the other, causing it to turn mid-flip.

To fix this issue, I tweaked the ratio between the motor speeds at which the car drives back away from the wall such that it can course-correct itself after it flips. Instead of sending the same PWM signal to both motors, I lowered one just enough to not veer off to the side, but not too much as to affect the reliability of my flips. I also added a very small delay to the motor that stops earlier, such that it runs a little longer to match the other.

[![Stunt_run_1](https://img.youtube.com/vi/yU46CBN-2M4/0.jpg)](https://www.youtube.com/watch?v=yU46CBN-2M4)

[![Stunt_run_2](https://img.youtube.com/vi/M9IMlJxRE2o/0.jpg)](https://www.youtube.com/watch?v=M9IMlJxRE2o)

[![Stunt_run_3](https://img.youtube.com/vi/AQgau0eaHMU/0.jpg)](https://www.youtube.com/watch?v=AQgau0eaHMU)

you can see it getting better! I added a slight delay to one of the wheels stopping, as well as added a difference in speeds when they start back up again to drive towards me.

![flip_tof_2](../images/Lab8/flip_tof_2.png)

![flip_motor_2](../images/Lab8/flip_motor_2.png)

There is still a bit of an unwanted pivot, but unfortunately the mat was not sticky enough to perform my stunt anymore after too many tries. So I will be back when I get a better calibration on another sticky mat, or perhaps a DIY mat if I can find materials.