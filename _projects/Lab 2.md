---
layout: project
title: Lab 2 - IMU
description: Lab 2
image: /assets/images/Lab 2/IMU.png
---

<h2> Lab Tasks </h2>

___

<h3> Set up the IMU </h3>

___

To setup the IMU, the “SparkFun 9DOF IMU Breakout_ICM 20948_Arduino Library” was downloaded from the Arduino Library Manager. The IMU is connected to the Artemis board by using a QWIIC connector. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 062119.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

To ensure that the IMU is working, the IMU example code was ran. Within the code, the AD0_VAL represents the status of the IMU breakout. The default is 1, and when the ADR jumper is closed, the value becomes 0. To test the functionality of the IMU, I rotated the board in my hand. It is observed that the acceleration and gyroscope data rapidly change when the board is quickly rotated and updates frequently. The gyro data measures in degrees per second, and the acceleration data measures in mg.

<iframe width="560" height="315" src="https://www.youtube.com/embed/YsKg19Vgcls?si=MnftygVmBenAz4dL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

To indicate that the board is running, I added code to have the board blink three times on start-up. I wrote the following code in the Arduino sketch:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 063055.png' | relative_url }}"
        width="400"
         alt="connect IMU">
</figure>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Sc3zBi1hLzM?si=rIKpP6CbrTSER3sG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<h3> Accelerometer </h3>

___


To convert the IMU data into pitch and roll measured in degrees, the following code was implemented:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 063753.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

The following video shows the output when held at between -90 and 90- degrees.

<iframe width="560" height="315" src="https://www.youtube.com/embed/vztnUfnBQZg?si=6LgZEPkqDHdIUQAe" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The accuracy of the accelerometer was not perfect and needs calibration. For pitch, when held at -90 degrees, the output is -89.5 degrees, and when held at 90 degrees, the output is 87 degrees. For roll, when held at -90 degrees, the output is -94 degrees, and when held at 90 degrees, the output is 90.5 degrees. The outputs are then used to create a 2-point calibration. 

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 065839.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

To post-process the data, the following command is written to collect roll and pitch data for a duration of 5 seconds. A notification handler was created in the Jupyter notebook to collect the data with associated timestamps.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070045.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070215.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

There is a decent amount of noise in the accelerometer sensors of the IMU. A FFT was used to analyze the noise.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070325.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070405.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070523.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 070550.png' | relative_url }}"
        width="600"
         alt="data">
</figure>

From the above figures, I decided to choose a cutoff frequency of 5 Hz, as much of the data points were collected within this range. The cutoff frequency greatly affects the amount of frequencies that is allowed to pass through and the amount of noise removed. It is important to choose a cutoff frequency that is low enough to remove high frequencies while maintaining sensor data. 

A low pass filter is implemented into the code. By using the equation:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 071117.png' | relative_url }}"
        width="300"
         alt="data">
</figure>

With a cutoff frequency of 5 Hz, and a sampling spacing of 0.01421 seconds, the alpha is 0.3087.

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 071817.png' | relative_url }}"
        width="500"
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 071833.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>


<h3> Gyroscope </h3>
___

To calculate pitch, roll, and yaw angles from the gyro, the following code is written:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072211.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

After rotating the IMU for a duration of 5 seconds, the following data was collected:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072304.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072338.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072353.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

Compared to the accelerometer, outputs from the gyro have significantly less noise but will drift within a short period of time. To make the data more accurate and stable, a complementary filter was implemented:

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072618.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

With an alpha value of 0.9, the filter created data that is stable against oscilations and vibrations (which I created by tapping the table).

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072740.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072826.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072841.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

<figure style="text-align:center">
    <img src="{{ '/assets/images/Lab 2/Screenshot 2026-02-18 072857.png' | relative_url }}"
        width="500  "
         alt="data">
</figure>

With a high alpha value, the filter relies more on the accelerometer data than the gyro data. I chose this value to minimize the amount of drift induced by oscillations of the sensor and from vibrations.

<h3> Sample Data </h3>
___

<h3> Record a Stunt </h3>
___

<iframe width="560" height="315" src="https://www.youtube.com/embed/2CQpFQ4ZwGI?si=yAPuuBDB043LORoL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The RC vehicle is capable of accelerating quickly back and forth. When traversing in one direction, a reverse input would cause the vehicle to flip over. With enough practice, you are able to make the RC dance in a tight spot. While not shown in the video, the RC is also capable of turning very quickly. 



<h2> Discussion </h2>

This lab teaches how to collect and process IMU data, such as calibration, fusing sensors to obtain more accurate data, and how to increase execution of code when handling large amounts of data. 


<h2> Collaborators </h2>

For Lab 2, I collaborated with Sean Zhen (sz378). I also used pages from last year as references from Selena Yao and Angela Voo. 