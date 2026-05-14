---
layout: default
title: "Lab 12"
permalink: /lab_documentation/lab_12/
description: "writeup for lab 12"
---


# LAB 12 - MAE4190 FAST ROBOTS

This is lab 12 of fast robots. In this lab, the task is open ended, so I have decided to try and attempt a local path planning procedure through the map that we had been given.

### Attempt at Feedback Control

I started out trying to use the positional PID controls with the TOF sensors. This was my workflow:
1. At each waypoint, a python function would calculate the angle towards the next waypoint, and an arduino command, upon receiving the required angle, would run PID orientation control to steer the car towards it:

```python
def targetAngle(curr_x, curr_y, next_waypoint):
        next_x = next_waypoint[0]
        next_y = next_waypoint[1]
        
        target_angle = math.atan2(next_y - curr_y, next_x - curr_x) * 180/pi
       
        return target_angle
```

```C++
    void
    waypointAngleControl(float waypoint_angle, float curr_angle, int index){
    //ENABLE DMP
    bool success = true;
    //PID CALCULATION
    PID_calculation(curr_angle, waypoint_angle, index);
    //motor input
    if (PID_doc[index] < 0){
        PID_leftward(PID_doc[index], index);
    } else {
        PID_rightward(PID_doc[index], index);
    }

    if (abs(error_doc[index]) <= 2){
        control_stop();
        error_too_big = false;
        delay(3000);
        // Reset FIFO
        success &= (myICM.resetFIFO() == ICM_20948_Stat_Ok);
    }
}
```

The arduino function is modified from the localization code, but with one set target angle instead of a gradually incrementing one.

2. Another python function will calculated the target distance that the car needs to travel towards the next waypoint, with which another arduino command would execute PID linear motion control to arrive at the next waypoint:

```python
def targetDistance(x, y, curr_dist, next_waypoint):
    
        next_x = next_waypoint[0]
        next_y = next_waypoint[1]
       
        distance = ( math.sqrt((x2 - x)**2 + (y2 - y)**2) ) * 304.8
       
        target_distance = curr_dist - distance

        return target_distance
```

I needed the TOF reading for this, to get the current distance, hence I added an arduino command for that too.

```C++
void
waypointDistControl(float target_dist, float curr_dist, int index){
    //PID CALCULATION
    PID_calculation(curr_dist, target_dist, index);
    //motor input
    if (PID_doc[index] > 0){
        PID_forward(PID_doc[index], index);
    } else {
        PID_backward(PID_doc[index], index);
    }

    if (abs(error_doc[index]) <= 2){
        control_stop();
        error_too_big = false;
        delay(3000);
    }
}
```

```C++
        case SEND_DIST_DATA: {

            float curr_tof = distanceSensor2.getDistance(); //Get the result of the measurement from the sensor
            distanceSensor2.clearInterrupt();

            tx_estring_value.clear();
            tx_estring_value.append(curr_tof);
            tx_characteristic_string.writeValue(tx_estring_value.c_str());

            break;
        }
```

My PID calculations and motor controls are the same as previous labs.

3. Since as seen from lab 11, the control of my car is fairly poor, so I chose not to trust calculated expected control inputs, and decided to localize at every waypoint. This did unfortunately make the entire path planning process incredibly slow, as it takes a long time for the car to localize as well as send data over through the BLE.

```python
    for i in range(len(waypoints)):

        target_angle = RealRobot.targetAngle(curr_pt[0], curr_pt[1], waypoints[i+1])
        print("target angle calculated: ", target_angle)
   
        ble.send_command(CMD.TARGET_ANGLE_TURN, f"0.8|0|0|{target_angle}")
        #Kp needs to be pretty small

        await asyncio.sleep(10)

        ble.send_command(CMD.SEND_DIST_DATA, "")
        curr_dist = float(ble.receive_string(ble.uuid['RX_STRING']))
        
        await asyncio.sleep(3)
        
        print("tof data sent: ", curr_dist)

        target_dist = RealRobot.targetDistance(curr_pt[0], curr_pt[1], curr_dist, waypoints[i+1])

        print("target distance calculated: ", target_dist)
        
        ble.send_command(CMD.TARGET_DIST_RUN, f"0.1|0|0|{target_dist}")
        # too fast

        await asyncio.sleep(5)

        #localization
        curr_pt = await RealRobot.localizeSpin(waypoints[i+1])
        #curr_pt = (waypoints[i+1])
        print("localized: ", curr_pt)
```

The TOF feedback controls proved difficult as the angular adjustments were not the most reliable and would often guide the TOF sensor within sight of some obstacles that does not match the expected distance we should be reading.

Here is a very bad first attempt:

[![bad_first_attempt](https://img.youtube.com/vi/3r3Guliwuts/0.jpg)](https://www.youtube.com/watch?v=3r3Guliwuts)

Moreover, similar to previous labs, my car's ability to localize reliably with an on-axis turn is not great. The localization at each waypoint introduced so much error and drift into its trajectory that the car would often lose its way. I decided to then reduce the number of waypoints at which it would localize.

```python
    if to_local[i]:
        curr_pt = await RealRobot.localizeSpin(waypoints[i+1])
    else
        curr_pt = (waypoints[i+1])
    print("localized: ", curr_pt)
```

Unfortunately ultimately I decided to forego all of the localization points, and it turned almost into an open loop control, but I think with more time to calibrate and smooth out any redundancies in my code, I might have been able to do it. I might also have benefitted from implementing a Kalman filter.

I had to increased the Kp by much more than I expected to get a faster result. The original cycle took me maybe at least ten minutes to run in full. The car wiggles a lot more during orientation control

### Final Pathing

[![full_run](https://img.youtube.com/vi/vxYq2N79kq8/0.jpg)](https://www.youtube.com/watch?v=vxYq2N79kq8)
