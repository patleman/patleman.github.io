---
title: "Perception Project"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios.<br/><img src='/images/cbf_diagram1_c1.jpg'>"
collection: portfolio
---


The overview of the developed algorithm is shown in the diagram below: 
<img src='/images/CBFcontroller (1).png'>

The equations used in the algorithm is shown in the diagram below:
<img src='/images/equation_controller_po.drawio.png'>



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

$$
\begin{aligned}
&\arg_{\delta_{ha} > 0} \min_{\dot{\boldsymbol{q}}_{safe}}\left\{||\dot{\boldsymbol{q}}_{safe}-\dot{\boldsymbol{q}}_{perf}||^2 + \beta  \delta_{ha}^2 \right\} \\
&\text { s.t.} \\
&A_{hej}(2J_{j}(\boldsymbol{x}) \ \dot{\boldsymbol{q}}_{safe}-\dot{\boldsymbol{p}}_{he})\geq - \alpha (h_{hej}(\boldsymbol{x},t)), \\
&A_{haj}(2J_{j}(\boldsymbol{x}) \ \dot{\boldsymbol{q}}_{safe}-\dot{\boldsymbol{p}}_{ha})\geq - \alpha (h_{haj}(\boldsymbol{x},t))-\delta_{ha}, \\
&\dot{\boldsymbol{q}}_{min} \leq \dot{\boldsymbol{q}}_{safe} \leq \dot{\boldsymbol{q}}_{max}, \\
&\quad j = 1, 2, \dots, n
\end{aligned}
$$



