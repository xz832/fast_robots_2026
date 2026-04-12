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

