---
layout: project
title: Lab 7 - Kalman Filter
description: Lab 7
image: /assets/images/Lab 7/Screenshot 2026-03-24 220523.png
---

<h2> 1. Estimate drag and momentum </h2>
___

During this lab, the distance sensor will give incorrect distance data when placed too far from the wall, so the furthest the robot can be placed from a wall is ~1.75 meters.

With this distance, for the robot to reach steady-state speed, I set the PWM value to 40, which is about 65% of the max PWM input obtained from lab 5. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-24 234845.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-24 235044.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 001012.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

The steady-state speed is 0.8103 m/s, the 90% rise time is 5.761 seconds, and the speed at this rise time is 0.7293 m/s.


<h2> 2. Initialize KF </h2>
___

From lecture, we know that the matrices A and B are defined as:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 002800.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

To find d, we can use the equation: 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 003223.png' | relative_url }}"
        width="100"
         alt="connect IMU">
</figure>

With u assumed to be 1 and steady-state velocity being 0.8103 m/s, `d = 1 / 0.8103 = 1.234`.


To find m, we can use the equation: 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 003531.png' | relative_url }}"
        width="200"
         alt="connect IMU">
</figure>

With the 90% rise time being 5.761 seconds, `m = 3.0875`

So, the A and B matrices are:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 004633.png' | relative_url }}"
        width="200"
         alt="connect IMU">
</figure>

The sampling time in which a new distance measurement is obtained from the ToF sensor is 103 ms. The following code shows the discretized matrices along with matrix C and the state vector, x. Matrix C is [1,0] as the distance from the wall is positive. 

<script src="https://gist.github.com/SamZhen67/e74ca08512bc4ec7878f67ada3aaba1e.js"></script>

For the Kalman Filter to work well, the process noise and sensor noise covariance matrices need to be specified. For process noise, with a sampling time of 103 ms, the standard deviation in position (sigma_1) is 31.16 mm, and the standard deviation in velocity (sigma_2) is 31.16 mm/s. For measurement noise, the ToF datasheet states that the short distance mode has an error of 20 mm, so sigma_3 is 20. The covariance matrices are implemented as shown: 

<script src="https://gist.github.com/SamZhen67/9dc22cc9a8257351392a7a077367e4c2.js"></script>

<h2> 3. Implement and test your Kalman Filter in Jupyter (Python) </h2>
___

Using the given Kalman filter example, the filter was implemented onto data from Lab 5: 
<script src="https://gist.github.com/SamZhen67/83f9179995f0b31eb383e6cc843fe6f4.js"></script>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 025340.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

The Kalman filter seems to estimate the system state very well with a small lag. After increasing process noise to a standard deviation of 80 for sigma_1 and sigma_2, the filter trusts the measurements more than the model, and the time shift between the filter and raw measurements decrease. 

<h2> 4. Implement the Kalman Filter on the Robot </h2>
___
Using the same command from Lab 5 to have the robot stop at the wall, the following code snippets were added into the command to implement the Kalman filter.
<script src="https://gist.github.com/SamZhen67/50c7d9cce49af649486388c9ce5761ae.js"></script>

Tha Kalman filter was able to estimate the state of the robot very accurately. With the Kalman filter in place, I was also able to run a more aggressive PID controller than lab 5.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 7/Screenshot 2026-03-25 045725.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

<iframe width="560" height="315" src="https://www.youtube.com/embed/UQiamtV9Rmo?si=jvSjXaBGnOMqTtL_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h2> Collaborators </h2>

For Lab 7, I referenced Aidan Derocher's work. 