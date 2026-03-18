---
layout: project
title: Lab 6 - Orientation Control
description: Lab 6
image: /assets/images/Lab 6/Screenshot 2026-03-17 180853.png
---


<h2> 1. Prelab </h2>
___
To set up a robust system for debugging the PID controller, I plan to send gains for my PID controller each time I run a closed-loop command. For this lab, I plan to send information for Kp, Ki, Kd, and the amount of time the robot is commanded before the robot "times out" and stops. This allows the robot to stop after a specified period of time whether or not the robot has Bluetooth connection.

This can be done by using the `ble.send_command` function in Python, which allows new gains to be updated each time without needing to upload new code. The following Arduino code shows the framework of the command for this lab: 

<script src="https://gist.github.com/SamZhen67/df89a03c6658cfce01a7fba65cbde4ae.js"></script>

Whenever a closed-loop command is requested, debugging data is stored in arrays and is then sent to a notification handler. The time-stamped debugging data that I chose to collect are sensor data from the IMU and the outputs of each branch from the PID controller. 

<script src="https://gist.github.com/SamZhen67/d36d7a6600cca1a93c57fdea1cc5ea51.js"></script>

<h2> 2. Lab Tasks </h2>
___

<h3> PID Input Signal </h3>

To get an estimate of the orientation of the robot, the gyroscope data `myICM.gyrZ()` will be integrated over time. 

<script src="https://gist.github.com/SamZhen67/da4528c8f683f1b30d56401a4ce49c83.js"></script>


While integrating gyroscope data has low noise, the problem that arises is that the readings will drift over time. 


PID Input Signal

    Are there any problems that digital integration might lead to over time? Are there ways to minimize these problems?
    Does your sensor have any bias, and are there ways to fix this? How fast does your error grow as a result of this bias? Consider using the onboard digital motion processor (DMP) built into your IMU to minimize yaw drift.
    Are there limitations on the sensor itself to be aware of? What is the maximum rotational velocity that the gyroscope can read (look at spec sheets and code documentation on github). Is this sufficient for our applications, and is there a way to configure this parameter?


Tuning steps:
I set a very aggressive P of 10 where it oscillates with a large disturbance and brought it down until it doesn't do it anymore. That value is somewhere below 5. 2.4 looks good for P


bring I up till it oscillates and then bring it back down.



P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
Range/Sampling time discussion
Graphs, code, videos, images, discussion of reaching task goal
Graph data should at least include theta vs time (you can also consider angular velocity, motor input, etc)



<h2> Tasks for 5000-level students </h2>
___
(5000) Wind-up implementation and discussion    


<h2> Collaborators </h2>

For Lab 5, I collaborated with Sean Zhen (sz378). 