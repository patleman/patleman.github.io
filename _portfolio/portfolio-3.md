---
title: "Trajectory Tracking Controller for SCARA Manipulator(MATLAB)"
excerpt: "This project discusses the formulation of torque-based control for the SCARA manipulator, along with trapezoidal velocity profile-based trajectory generation.<br/><img src='/images/cbf_diagram1_c1.jpg'>"
collection: portfolio
---
Image Source:(https://howtorobot.com/expert-insight/industrial-robot-types-and-their-different-uses)

Problem Statement:
### Problem Statement:

1. **Trajectory Generation in Operational Space with Trapezoidal Velocity Profile**:  
   Generate a trajectory for a robotic arm in its operational space over 4 seconds with a trapezoidal velocity profile. The trajectory should pass through the following waypoints:  
   - **p0 = [0, -0.80, 0]** at **t0 = 0.0**  
   - **p1 = [0, -0.80, 0.5]** at **t1 = 0.6**  
   - **p2 = [0.5, -0.6, 0.5]** at **t2 = 2.0**  
   - **p3 = [0.8, 0.0, 0.5]** at **t3 = 3.4**  
   - **p4 = [0.8, 0.0, 0.0]** at **t4 = 4.0**  
   The robot should not stop at any waypoint, with each serving as a via point. The anticipation time between each segment is 0.2 seconds, and the trajectory should be sampled with a time step of **Ts = 0.001 s**. Compute and plot the **position, velocity, and acceleration** profiles of the trajectory for each segment.

2. **Inverse Dynamic Control Approach with 5 kg Load**:  
   Using the trajectory generated in the operational space, implement a **second-order inversion kinematic algorithm** to generate the joint setpoints for a robot arm with a 5 kg load placed at the end effector. Develop an inverse dynamic control approach to calculate the required joint torques or forces based on the generated trajectory and joint setpoints. The control strategy should ensure that the robot follows the path accurately while accounting for the load dynamics.




