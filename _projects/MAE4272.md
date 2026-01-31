---
layout: project
title: FA25 - MAE 4272 - Blade Design
description: Design Project
technologies: [Autodesk Fusion, MATLAB]
image:  '/assets/images/project.png'
---

<h3> Project Overview </h3>
In MAE 4272 - Fluids and Heat Transfer Laboratory, our team was tasked with designing and testing a wind turbine blade optimized for power generation under a specific Weibull wind distribution with a mean speed of 4.59 m/s. The goal was to maximize power output at a set rotation rate while adhering to strict geometric, material, and operational constraints, including a maximum blade length of six inches and a torque limit of 3.5 N·cm.

<h3> Design Process </h3>
We selected the NACA 4412 airfoil for its wide Reynolds number range and modeled the blade using 10 radial sections. Using MATLAB, we performed sweeps across pitch angles and rotation rates to optimize chord length, twist, and induction factors—accounting for wake rotation and aerodynamic forces. Our analysis identified 800 RPM as the optimal operating speed, predicting 2.666 W of power. A stress analysis confirmed the blades could withstand winds up to the 95th percentile without failure.

<figure>
    <img src="{{ '/assets/images/dist.png' | relative_url }}"
        width="400"
         alt="Weibull wind distribution">
    <figcaption>Figure 1: Weibull wind distribution used for design optimization</figcaption>
</figure>

<figure>
    <img src="{{ '/assets/images/blade.png' | relative_url }}"
        width="400"
         alt="CAD">
    <figcaption>Figure 2: Final blade CAD model showing optimized twist and chord distribution</figcaption>
</figure>

<h3> Testing Summary </h3>
We experimentally validated the design by generating power curves at three wind speeds: 5th percentile (3.0 m/s), mean (4.6 m/s), and 95th percentile (6.3 m/s). Testing was conducted in the Big Blue wind tunnel using a torque brake to measure performance across varying loads. Results showed strong agreement in optimal RPM (750 RPM experimental vs. 800 RPM theoretical, 6.25% error), though power output was lower than predicted due to surface roughness and unmodeled tip losses.

<figure>
    <img src="{{ '/assets/images/curve.png' | relative_url }}"
        width="400"
         alt="Weibull wind distribution">
    <figcaption>Figure 3: Experimental power curves showing peak performance at each wind speed</figcaption>
</figure>

<h3> My Contribution </h3>
I created the MATLAB script for the blade optimization, performed stress analysis to ensure structural integrity, and contributed to experimental setup and data collection. I also coordinated team deliverables and assisted in the final reporting and presentation.