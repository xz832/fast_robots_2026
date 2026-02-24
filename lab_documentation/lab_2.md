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

In order to broadcast the data over to python (gyroscope code included, but they are similar):

```python
timestamp = []
acc_pitch = []
acc_roll = []
gyr_pitch = []
gyr_roll =[]
gyr_yaw = []

def notifyBle(uuid, data):
    data = data.decode().strip()
    parts = data.split(",")
    
    timestamp.append(float(parts[0]))
    acc_pitch.append(float(parts[1]))
    acc_roll.append(float(parts[2]))
    gyr_pitch.append(float(parts[3]))
    gyr_roll.append(float(parts[4]))
    gyr_yaw.append(float(parts[5]))
```

Graphing the accelerometer data:

```python
plt.plot(timestamp, acc_pitch[0:3000], label="Pitch")
plt.plot(timestamp, acc_roll[0:3000], label="Roll")
        
plt.xlabel("Time")
plt.ylabel("Pitch / Roll")
plt.title("Accelerometer Pitch and Roll vs Time")
plt.legend()
plt.grid(True)
        
plt.show()
```

Here is the jupyter graph that generates with the data obtained with an arbitrary orientation. As the graph shows, the data is quite noisy.
![acc_graph](../images/Lab2/acc_data_graph.png)

Here is a graph that shows the accelerometer data while I'm flipping the IMU around. The sudden spike at the end is due to me setting the IMU down on the table.
![acc_flip_graph](../images/Lab2/acc_flip_graph.png)

In order to filter out some of the noise, we want to analyze the frequencies at which the noise occurs at with a Fourier Transform.

For the accelerometer roll Fourier Transform:
```python
dt = np.mean(np.diff(timestamp)) / 1000

N = len(acc_roll)
fft_roll = np.fft.fft(acc_roll)
freqs_roll = np.fft.fftfreq(N, d=dt)

pos_freq = freqs_roll > 0

freqs_roll = freqs_roll[pos_freq]
fft_mag = np.abs(fft_roll[pos_freq])

plt.plot(freqs_roll, fft_mag)
plt.xlabel("Frequency (Hz)")
plt.ylabel("Amplitude")
plt.title("FFT of accelerometer roll")
plt.grid(True)
plt.show()
```

![fft_acc_roll](../images/Lab2/fft_acc_roll.png)

For the accelerometer pitch Fourier Transform:
Code is very similar, except with the pitch data from the accelerometer instead.

![fft_acc_pitch](../images/Lab2/fft_acc_pitch.png)

Looking at the FFT graphs, the accelerometer pitch shows significant peaks after around 3.7Hz. Hence, that will be my benchmark for the low pass filter. In order to calculate the alpha:

```python
low_pass_freq = 3.7
alpha = (2*np.pi*low_pass_freq*dt) / (1 + 2*np.pi*low_pass_freq*dt)

filtered = np.zeros_like()
filtered[0] = theta[0]

for i in range(1, len(theta)):
    filtered[i] = alpha*theta[i] + (1-alpha)*filtered[i-1]

plt.plot(times, theta, label="Raw")
plt.plot(times, filtered, label="Filtered")
plt.legend()
plt.show()

```

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