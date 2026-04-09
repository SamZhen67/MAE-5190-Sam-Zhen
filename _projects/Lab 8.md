---
layout: project
title: Lab 8 - Stunts
description: Lab 8
image: /assets/images/Lab 8/Screenshot 2026-04-08 210622.png
---

For this lab, my robot will be conducting *Task B: Drift*. To complete a drift, the robot will complete 3 different steps:
1. Using open-loop control, drive towards the wall until a target distance is reached.
2. Once the target distance is reached, start a differential turn in which the right motor spins forward while the left motor spins backwards for a set power and time. 
3. After the turn is complete, request each motor to drive forwards away from the wall. 

<h2> Step 1: Drive forwards </h2>

<script src="https://gist.github.com/sam-zhen/3bd5c8421f36c0c3c35c7dea79047aee.js"></script>

The `distance_to_turn` is 1300 mm, the `output_straight` PWM value is 90, and the `CALIBRATION_FACTOR` is 0.82 to ensure that the robot drives straight.

The `turn_flag` and `turning` variables are first initialized as `false`, which indicates that the robot has not reached the conditions to start the turn. When the distance from the ToF sensor is less than `distance_to_turn`, `turn_flag` and `turning` both change to `true`. With the `turn_flag` set to `true`, the if statement will not run again and ignores any future distance readings that are less than `distance_to_turn`. This prevents the robot from turning more than once during the stunt. 

<h2> Step 2: Drift </h2>

<script src="https://gist.github.com/sam-zhen/33cc55c3d1846802800e7f08ef655048.js"></script>

The `output_turn` PWM value is set to 120 with the same `CALIBRATION_FACTOR`, and `delay_time` is set to 330 ms. `delay_time` is the variable that controls the amount of time the robot spends drifting using a differential turn. 

Once the amount of time since the start of the turn has exceeded `delay_time`, the `turning` variable is set to `false` to exit the turning state. 

<h2> Step 3: Drive away from wall </h2>

Once the turn is completed, the robot returns to the FORWARD STATE, which is the same state as step 1.

<h2> Videos </h2>

In each video, the robot starts approximately 3 meters from the wall due to space constraints. The neon yellow tape indicates 3 feet from the wall. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/6akg-53b5PA?si=WhdVjTmvAz121zoC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/-y0gWjh-oFE?si=Urhhf1yKhDFbnXbK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/OtbvgTutDFE?si=d0dRmNoAyEi8Z5NM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h2> Graphs </h2>

The provided graphs shows the distance sensor readings and the motor PWM inputs from one run. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 8/Screenshot 2026-04-08 220055.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

The vertical dashed line indicates the time of 125050 ms in which the distance sensor detected a distance that is less than 1300 mm. This graph also shows the effect of the `turn_flag` preventing the robot from conducting more than one drift. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 8/Screenshot 2026-04-08 220032.png' | relative_url }}"
        width="500"
         alt="connect IMU">
</figure>

The blue vertical line shows a `delay_time` of 330 ms after the turn has started. The two different PWM values shown as the robot is driving forwards shows the effect of the `CALIBRATION_FACTOR` to ensure that the robot drives straight. To make a differential turn, the left motor must rotate backwards while the right motor continues to rotate forwards. 

<h2> Discussion </h2>

By not using a closed-loop PID control and instead using open-loop control, PWM values and `delay_time` can be easily chosen to allow the robot to complete a drift. Each of the three runs were successful as the robot did not hit the wall and made a 180-degree turn. Only the distance sensor is used to perform this stunt as this allows less variables that need to be tuned. While a IMU can provide more accurate turns by using the gyro data, it was not necessary as the `delay_time` can be adjusted to complete a 180-degree turn. A Kalman filter would also allow the robot to predict the distance from the wall given few distance readings, but was also not necessary as the robot reacts quickly to not hit the wall by setting a large enough `distance_to_turn` value. 

<h2> Collaborators </h2>
For Lab 8, I worked with Sean Zhen. 