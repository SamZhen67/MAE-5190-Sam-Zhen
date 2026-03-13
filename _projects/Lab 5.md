---
layout: project
title: Lab 5 - Linear PID
description: Lab 5
image: /assets/images/Lab 5/PID_en.svg.png
---

<h2> 1. Prelab </h2>
___

To set up a robust system for debugging the PID controller, I plan to send gains for my PID controller each time I run a closed-loop command. For this lab, I plan to send information for Kp, Ki, Kd, and the amount of time the robot is commanded before the robot "times out" and stops. This allows the robot to stop after a specified period of time whether or not the robot has Bluetooth connection.

This can be done by using the `ble.send_command` function in Python, which allows new gains to be updated each time without needing to upload new code. The following Arduino code shows the framework of the command for this lab: 

<script src="https://gist.github.com/SamZhen67/7c46d57a61357528e2668f629a8cb2f2.js"></script>

Whenever a closed-loop command is requested, debugging data is stored in arrays and is then sent to a notification handler. The time-stamped debugging data that I chose to collect are sensor data from the ToF sensors and the output from the PID controller. 

<script src="https://gist.github.com/SamZhen67/5a78cc94359070d5f7fad8fd505c37da.js"></script>

<h2> 2. Lab Tasks </h2>
___

<h3> 1. Position Control </h3>

From lab 4, we know that the maximum PWM value for the motors is 255. To determine an appropriate limit for the proportional controller, we can use the maximum expected error. As the ToF sensors are currently operating in short range mode, the maximum range is 1.3 meters, and we expect the robot to stop at a range of 0.304 meters, which gives a max error of 0.996 meters. With this error, the maximum Kp is 255 / 996 mm = 0.256.

The following code shows the implemented PID controller:

<script src="https://gist.github.com/SamZhen67/bde285e4acc2c40114781a94bb85efb1.js"></script>

A PID controller was used to ensure that as much steady-state error was removed from the final position of the robot and to ensure smooth motion during closed-loop control. The values of the PID controller are `Kp = 0.045, Ki = 0.11, Kd = 0.08`. A low-pass filter was used on the derivative controller to smooth the input from this branch with `a = 0.1`. This value was empirically chosen to generate a derivative controller with no rapid velocity changes. Also, derivative kick was eliminated by ignoring the first reading to prevent a large change in error for the derivative branch. 

To test the robustness of the controller, the robot was set at three different distances within the range of the ToF sensor. The first test was conducted at 4 feet, and the following tests are conducted at three feet and then two feet.

<u> Test #1: 4 feet </u>

<iframe width="560" height="315" src="https://www.youtube.com/embed/40ntIpv2BJU?si=7e-9WH0OJrUXzjUX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 025916.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 025937.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>


<u> Test #2: 3 feet </u>

<iframe width="560" height="315" src="https://www.youtube.com/embed/fK1u4BfCuHs?si=wH0OxHPk_A4sN2ik" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 030029.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 030058.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>


<u> Test #3: 2 feet </u>

<iframe width="560" height="315" src="https://www.youtube.com/embed/7yTLZslqub4?si=7pwQJgwNvvjs84Lk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 030120.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>


<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 030135.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<u> Max achieved speed: </u>

The max achieved speed was during test #1 in which an average of 0.5 m/s.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 031733.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>   

<h3> 2. Extrapolation </h3>

The frequency that the ToF sensor is returning new data is about 10 Hz (~100 ms), which is currently limiting how frequent the PID loop updates. To decouple these two rates, the PID loop should update whether or not there is new data from the ToF sensor. This is done by removing the `while (!distanceSensor1.checkForDataReady()) {delay(1); } `. In 2839 ms, 1385 updates were given, so this equal to a frequency update of 487 Hz, which is 48 times faster than the previous command. 

The following was written in Python to extrapolate new ToF values based on recent sensor values. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 043005.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>   

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 043054.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>   


<h2> Tasks for 5000-level students </h2>
___

Integrator windup protection is required to prevent PID controllers from accumulating massive, excessive error signals when actuators saturate. This was implemented in my controller by creating a limit on how much the integrator controller can contribute to the overall output of the PID controller.

<script src="https://gist.github.com/SamZhen67/4de7272fcd3a6ab17888fbe55fd32c2e.js"></script>

A clamp value of 30 was chosen, which helped limit the strength of the integrator controller. This can be seen in the highlighted region in the following figure.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 024500.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

Without windup protection, the integrator controller dominates the PID loop and causes the vehicle to overshoot the setpoint. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/wbh3c489EjA?si=P6TY8guLfFeG-Sv0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 5/Screenshot 2026-03-13 041214.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<h2> Collaborators </h2>

For Lab 5, I collaborated with Sean Zhen (sz378). 


