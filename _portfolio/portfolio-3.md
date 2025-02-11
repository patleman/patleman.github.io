---
title: "Trajectory Tracking Controller for SCARA Manipulator(MATLAB)"
excerpt: "This project discusses the formulation of torque-based control for the SCARA manipulator, along with trapezoidal velocity profile-based trajectory generation.<br/><img src='/images/scara_animation.gif'>"
collection: portfolio
---
<img src='/images/Scara.jpg'>
Image Source:https://howtorobot.com/expert-insight/industrial-robot-types-and-their-different-uses

[Code](https://github.com/patleman/SCARA_MANIPULATOR-dynamics)

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

### Objective
1. Trapezoidal velocity profile based 3d-trajectory generation.
2. Second Order inverse kinematics to generate joint acceleration, velocity and position profile.
3. Derive Euler Lagrange dynamic equation for the Scara Manipulator.
4. Inverse Dynamic Controller (torque-control) for SCARA dynamical system.
5. Implementation using MATLAB

### Method
1. Code for trapezoidal velocity profile based 3d-trajectory generation, Second order inverse kinematics and derivations for Euler Lagrange dynamic equation can be found [here](https://github.com/patleman/SCARA_MANIPULATOR-dynamics).

There are five waypoints through which the trajectory of the end effector needs to be generated. Considering $$P_e$$ as the position vector of the end effector of the robot and $$P_0, P_1, P_2, P_3 $$, and $$P_4$$ as the position vectors of the waypoints, the following expression can be written:

$$
P_e = P_0 + \sum_{j=1}^{4} \frac{S_j}{\left\| P_j - P_{j-1} \right\|} (P_j - P_{j-1})
$$
where \(j = 1, 2, 3, 4\) refers to the \(j\)-th segment between \(P_j\) and \(P_{j-1}\) points. The variable \(S_j\) is used to provide the timing law to the path defined by the above equation. As per the question, \(S_j\) should be a trapezoidal velocity profile. Also, it is required to have an anticipation time before the start of each new segment. Considering all these required features, \(S_j\) is defined as follows:

$$
S_j(t) = 
\begin{cases} 
0 & \quad 0 \leq t \leq t_{j-1} - \Delta t_{j} \\
S_j'( t + \Delta t_{j}) & \quad t_{j-1} - \Delta t_{j} \leq t \leq t_{j} - \Delta t_{j} \\
d & \quad t_{j} - \Delta t_{j} \leq t \leq t_{f} - \Delta t_{N}
\end{cases}
$$
where $$\(\Delta t_{j} = \Delta t_{j-1} + \delta t_{j}\)$$ is the advance time at which the $$\(j\)$$-th segment starts and $$\(\delta t_{j}\)$$ is the anticipation time given in the question as 0.2 seconds, with \(\Delta t_{0} = 0\). Also, the trapezoidal profile for \(S_j'\) is defined as follows:

\[
S_j(t) = 
\begin{cases} 
S_i + 0.5 \times \ddot{q_c} t^2 & \quad 0 \leq t \leq t_c \\
S_i + \ddot{q_c} t_c \left( t - \frac{t_c}{2} \right) & \quad t_c \leq t \leq t_f - \Delta t_c \\
S_f - \frac{1}{2} \ddot{q_c} (t_f - t)^2 & \quad t_f - t_c \leq t \leq t_f
\end{cases}
\]
where \(t_c = \frac{t_f}{2} - \frac{1}{2} \sqrt{\frac{t_f^2 \ddot{q_c} - 4(q_f - q_i)}{\ddot{q_c}}}\), and \(\ddot{q_c}\) must be greater than or equal to \(\frac{4(q_f - q_i)}{t_f^2}\).

Using equations \((\ref{1})\), \((\ref{2})\), and \((\ref{3})\), the trajectory in 3D space is generated.

In part 2 of the project, the dynamics of the SCARA manipulator are derived using the Lagrangian equation. At the end, the following equation is derived:

\[
B(q)\ddot{q} + n(q,\dot{q}) = \tau
\]
where \(n(q,\dot{q}) = c(q,\dot{q})\dot{q} + F\dot{q} + g(q)\), \(B(q)\) is the 4x4 matrix, and \(C(q)\) is also a 4x4 matrix. The \(F_v\) matrix corresponds to the viscous force matrix for the motors, and \(g(q)\) represents the potential energy contribution towards the dynamic equation of the SCARA manipulator.

As per the question, inverse dynamics control in joint space is to be implemented. The model-referenced control law for the same is defined as follows:

\[
u = B(q)y + n(q,\dot{q})
\]

where

\[
y = K_d \ddot{\widetilde{q}} + K_p \widetilde{q} + \ddot{q}_d
\]

Here, \(K_d\) and \(K_p\) are positive definite diagonal matrices to ensure stability. \(\widetilde{q}\) is the error between the desired and actual joint values.

Input values for the desired joint positions, velocities, and accelerations are derived from the Second Order Inverse Kinematic Algorithm.

The desired translational trajectory for this Second Order Inverse Kinematics is taken from the trajectory generated in part 1 of this report.


3. Second Order Inverse Kinematics:
 <img src='/images/inverse_kinematics.jpeg'>
4. Inverse Dynamic Controller:
 <img src='/images/control.jpeg'>

### Result
<img src='/images/3d_part1.jpg'>
<img src='/images/Joint_position_part2.jpg'>
<img src='/images/X_trajectory.jpg'>
<img src='/images/Y_trajectory.jpg'>
<img src='/images/Z_trajectory.jpg'>
<img src='/images/scara_animation.gif'>

  

 
   




