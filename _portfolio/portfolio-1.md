---
title: "Control Barrier Function based Prioritized Obstacle Avoidance for Robotic Manipulator"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios.<br/><img src='https://github.com/user-attachments/assets/9e04538a-1641-44db-8089-bf51d310f37a' alt='CBF Implementation GIF'>"
collection: portfolio
---

## Problem Statement

In a human-robot collaborative environment, safety is a critical concern, alongside the performance of the robotic system. This project proposes a solution to address these challenges using the **Control Barrier Function (CBF)** theory as a mathematical tool and a **Depth Stereo ZED2i Camera** as the sensory input.

Additionally, a **cobot** works alongside the robot, so functionality is required to ensure that the robot avoids sensitive human body parts but is allowed to make contact with engaging parts, such as the hands. However, there is a possibility that an engaging body part might approach the manipulator at high velocity. In such scenarios, it is crucial for the algorithm to detect this and ensure collision avoidance.
<img src='https://github.com/user-attachments/assets/9e04538a-1641-44db-8089-bf51d310f37a' alt='CBF Implementation GIF'>"
## Objectives

1. **Set up the manipulator system and ZED2i camera.**
2. **Implement motion planning** for the following trajectories:
   - a) Pick and place operation
   - b) X-Y circular trajectory
   - c) Y-Z circular trajectory
   - d) Square-wave trajectory
3. **Design a PD controller** to achieve trajectory tracking.
4. **Design and implement a safety filter** using time-varying Control Barrier Functions (CBFs).
5. **Augment prioritization behavior** for different collision risks associated with various body parts.
6. **Analyze the collision avoidance behavior** for the aforementioned trajectories.

<br/><img src='/images/cbf_diagram1_c1.jpg'>"
## Advantages of the Solution

1. **Reduced computation time** compared to collision-avoidance solutions implemented at the motion planning stage.
2. **Forward-invariant safe-set** when compared with other Nonlinear Model Predictive Control (NMPC)-based solutions. While NMPC ensures the robot's state moves toward a defined safe set, it does not guarantee that the robot will remain in it over time. In contrast, the CBF-based solution makes the safe set forward-invariant, meaning if the system is in the safe set at some instance \(t_0\), it will remain in the safe set for all future times \(t_k > t_0\).
3. For complex scenarios where other NMPC-based optimization problems may fail to provide a solution, the CBF-based approach guarantees a solution by using a slack variable in the defined constraints.

                              

## Method
The overview of the developed algorithm is shown in the diagram below: 
<img src='/images/CBFcontroller (1).png'>

The equations used in the algorithm is shown in the diagram below:
<img src='/images/equation_controller_po.drawio.png'>

## Result

A simulation was performed for a 2D manipulator to
highlight the effect of the relaxation variable. The first diagram
shows the initial configuration of the manipulator and two obstacles.
The second diagram corresponds to the case where no relaxation
variable is used. The third figure corresponds to the case where the
red obstacle is prioritized more than the green obstacle. The fourth
diagram represents the plot of the relaxation variable value for the
previous case, capped at 0.6. The fifth diagram corresponds to the
case where the upper bound on the relaxation variable is kept high.

<img src='/images/highlighting_prioritization (2) (1).png'>

Plots of the relative velocity of the head and hand versus
their distance from the end-effector, based on data from 40 runs of
the experiment. A larger gap in the bottom-left corner of the head
plot, compared to the hand plot, indicates the prioritization of the head over hand. 

<img src='/images/v_vs_d.svg'>


Video below represents the implementation of the algorithm
<!-- Embed local video -->
<video width="640" height="360" controls>
  <source src="/images/CBF_implementation.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>



