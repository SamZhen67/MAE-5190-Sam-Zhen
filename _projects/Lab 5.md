---
layout: project
title: Lab 5 - Linear PID
description: Lab 5
image: /assets/images/Lab 5/PID_en.svg.png
---

<h2> 1. Prelab </h2>
___

To set up a robust system for debugging the PID controller, I wrote the following three commands in my Arduino sketch to start collecting PID data, stop collecting data, and to update gains.

<script src="https://gist.github.com/SamZhen67/a0147ff0926b715ebc6e8d9f0f202511.js"></script>

Whenever a closed-loop command is requested, debugging data is stored in arrays and is then sent to a notifcation handler. The time-stamped debugging data that I chose to collect are sensor data from the ToF sensors and the output from the PID controller. To make sure that I don't exceed the internal RAM of 384 kB, I plan to only have the command run for only 5 seconds. This also allows the robot to "time-out" and stop any robot control in case the Bluetooth connection fails on the robot. 

The following snippet shows the notification handler set to collect data from the robot:
<script src="https://gist.github.com/SamZhen67/0a05f5e8f7bdb55746aeb24ffe96d730.js"></script>

<h2> 2. Lab Tasks </h2>
___

To create PWM outputs from the PID controller, a function was created to use given kP, kI, and kD values.

The maximum output of the PID controller is set to 255, and the minimum is set to 20, which is under the lowest PWM value that keeps the robot moving. 



Consider the range and sampling time you choose for your TOF sensor; it may be worth lowering the accuracy for faster updates. Note that the medium range is only available if you are using the (ToF Pololu library).

Also note that the sensor has a programmable integration time. If this is set too high, you will see large jumps in your data as the robot drives and you can no longer assume that the measurements are independent. You can lower the integration time (trading off accuracy for speed) using: proximitySensor.setProxIntegrationTime(4); //A value of 1 to 8 is valid. Again this function is only available in the Tof Pololu library.

For this task, you will have your robot drive as fast as possible (given the quality of your controller) towards a wall, then stop when it is exactly 1ft (=304mm=1 floor tile in the lab) away from the wall using feedback from the time of flight sensor. Your solution should be robust to changing conditions, such as the starting distance from the wall (2-4m). If you attempt to do this at home, you could also show that your solution is robust to changing floor surface, e.g. linoleum or carpet. The catch is that any overshoot or processing delay may lead to crashing into the wall. You must also demonstrate that your controller is robust to external perturbations. If you push the car further from the wall, and then towards the wall, it should return to the desired setpoint


extrapolation

In Lab 7, you will learn how the Kalman Filter works and how you can implement this on your robot and use it to speed up sampling of the estimated distance to the wall. However, getting the Kalman Filter to work in practice takes time. A simple but less accurate alternative is a data extrapolator.

Write a function to extrapolate new TOF values based on recent sensor values, such that you can drive your robot quickly towards the wall with high accuracy. Be sure to demonstrate that your solution works by uploading videos and figures that plot corresponding raw and estimated data in the same graph.

Instructions:

    Determine the frequency at which the ToF sensor is returning new data.
        This is likely the rate at which your PID control loop is running as well. We want to decouple these two rates.
    Change your loop to calulate the PID control every loop, even if there is no new data from the ToF sensor.
        Check if new data from the ToF sensor is ready. If it is, update the variable that PID controller is using to estimate the motor speed.
        If a new datapoint isn’t ready, recalculate the PID control using using the last saved datapoint.
        The net effect this should have on your system should be the same. You PID control should now be running faster than your ToF sensor is generating new data.
    How fast is the PID control loop running? Compare this rate to ToF sensor rate.
    Rather than use an old datapoint to calulate the PID control, extrapolate an estimate for the car’s distance to the wall using the last two datareadings from the ToF sensor.
        Calcuate the slope from the last two datapoint, and extrapolate the current distance based on the ammount of time that has passed since the last reading and the slope.
        This is a simple linear extrapolation algorithm. Everytime you get a new ToF reading, use it along with the previous reading to estimate the current distance to the wall untill a new reading is recieved. If you have any questions about this, please ask one of the TAs for clarification.



    P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
    Range/Sampling time discussion
    Graphs, code, videos, images, discussion of reaching task goal
    Graph data should include Tof vs time and Motor input vs time (and whatever helps with debugging)
    (5000) Wind-up implementation and discussion

