---
layout: project
title: Lab 4 - Motors and Open Loop Control
description: Lab 4
image: /assets/images/Lab 4/Screenshot 2026-03-10 232055.png
---

<h2> 1. Prelab </h2>
___

With the A2 pin occupied by one of the ToF's XSHUT pin, I decided to wire the inputs of the right motor driver to pins A0 and A1, and the inputs of the left motor driver to pins A14 and A15. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-10 235333.png' | relative_url }}"
        width="600"
         alt="connect IMU">
</figure>

The Artemis and the motor drivers are powered by separate batteries because we don't want the Artemis board to brownout or lose enough voltage to not send or receive any data from other peripheral components. This would happen anytime the motors draw current if one battery is used.

<h2> 2. Lab Tasks </h2>
___

<h3> 1. Connect the necessary power and signal inputs to one dual motor driver  </h3>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-11 000254.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

At this point, we want to keep the motor driver powered by an external power sypply. The reasonable settings for the power supply is 3.7 V as it matches the nominal voltage of the battery powering each motor driver. 

<h3> 2. Use analogWrite commands to generate PWM signals  </h3>

The following code was writtent to generate a PWM signal to power the first motor driver:

<script src="https://gist.github.com/SamZhen67/cec3a03df1e2799199b5a4e827f12eeb.js"></script>

The following figure shows the motor driver connected to the oscilloscope and power supply:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-11 001946.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

The following video shows the read signal on the oscilloscope after running the `GENERATE_PWM` command:

<iframe width="560" height="315" src="https://www.youtube.com/embed/e029BRzckR4?si=hckj1Ps73JcikJVg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> 3. Take your car apart!  </h3>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-11 003104.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<h3> 4. Show that you can run the motor in both directions  </h3>

The following code was writtent to run the motor in both directions:

<script src="https://gist.github.com/SamZhen67/34240f5beb36c586016b36e3e2fae7ef.js"></script>

<iframe width="560" height="315" src="https://www.youtube.com/embed/LFYcgGMcGdA?si=L5J3544Vw5VMlS-8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> 5. Power the motor driver from the 850mAh battery instead of the power supply  </h3>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3QhQyaHxMiM?si=qP2VQDnRuWxRFoAP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> 6. Repeat the process for the second motor and motor driver  </h3>

Each previous step was repeated to ensure that the second motor and motor driver was integrated correctly to the rest of the car. An oscllioscope was used to check that clean PWM signals were being sent to the motor driver and that the motor was able to move forward and backwards.

<h3> 7 & 8. Install everything inside your car chassis, and try running the car on the ground; explore lower limit in PWM </h3>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-11 005103.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

After installing everything into the car, the lower limit for a PWM signal was explored. For my car, the lower limit is 28.

<iframe width="560" height="315" src="https://www.youtube.com/embed/kZu4O-_Ocd4?si=BIHzyqAMGgCUWw5m" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> 9. Implement a calibration factor </h3>

To ensure that the car drives straight, a 48/50 power split was created between the right and left motors.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 4/Screenshot 2026-03-11 005412.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Zo3fEL9y9Yk?si=Tbf1ud5kpTa4wEy0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> 10. Open-loop control</h3>

The following code and video shows open loop, untethered control of my robot.

<script src="https://gist.github.com/SamZhen67/febcb7068928a286890fdb9e7172d951.js"></script>

<iframe width="560" height="315" src="https://www.youtube.com/embed/9WJjsSsTF_U?si=xMG4e_22ZtDltSAK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h2> Additional tasks for 5000-level students </h2>
___

1) From the video attached to task 2, the oscilloscope shows that the frequency that analogWrite generates is about 183.3 Hz. A PWM signal at this speed is considered to be slow as motor drivers can often have input frequencies in the kHz range. The frequency of a PWM signal affects how smoothly the power is delivered to the motor. If the frequency is high, the motor sees a smooth voltage equivalent to the average of the pulses, rather than seeing the individual pulses, due to the motor's inductance. There are benefits of increasing the frequency, such as reducing audible noise, smoothing out motor torque, and reducing current ripple by keeping the current draw steady. 

2) After having the robot roll forward for a period of two seconds at a PWM value of 28, the lowest value that keeps the robot moving is 25. 

<script src="https://gist.github.com/SamZhen67/676ff58cae88db77a85a36d2e8653719.js"></script>

<iframe width="560" height="315" src="https://www.youtube.com/embed/_dW6cvQY9L8?si=zcSHkAGcEl-Io5mI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h2> Collaborators </h2>

For Lab 4, I collaborated with Sean Zhen (sz378). I also referenced to Aidan Derocher's and Angela Voo's pages.