---
title: "Implement a vision based 3-D pose estimator which estimates position and
orientation of the quadrotor based on AprilTags"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios.<br/><img src='/images/cbf_diagram1_c1.jpg'>"
collection: portfolio
---






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





