---
layout: default
title: "Lab8"
permalink: /lab_documentation/lab_8/
description: "writeup for lab 8"
---


# LAB 8 - MAE4190 FAST ROBOTS

Welcome to lab 8 of fast robots! In this lab we will be doing stunts with our cars!

I chose to do the flip

code:

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


interestingly at almost max PWM the two motors run pretty similarly, I originally had the 1.4 ratio between them, but it just veered off to the side. I did resolder one of the motor input wires between this lab and the previous few since the wire broke, so it could have been bad connection as well?

Made a sticky mat
Added bearing to front of car

flipped starting position since back of car too heavy by itself: would not flip even hitting a wall

Added a second bearing on the car
taped the back wheels for less friction
increased speed


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
        }
```

![hard_code_flip](../images/Lab8/hard_code_flip.gif)

[<img src="../images/Lab8/hard_code_flip.gif" width="30" />](../images/Lab8/hard_code_flip.gif)



### Distance based flip

![sideways](../images/Lab8/sideways.gif)

it likes to turn sideways a little bit after flipping, I think because the speed at which the two wheels stop are different, one stops slower than the other, causing it to turns