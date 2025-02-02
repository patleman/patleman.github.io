---
title: "Control Barrier Function based Prioritized Obstacle Avoidance for Robotic Manipulator"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios.<br/><img src='/images/cbf_diagram1_c1.jpg'>"
collection: portfolio
---

This is an item in your portfolio. It can be have images or nice text. If you name the file .md, it will be parsed as markdown. If you name the file .html, it will be parsed as HTML. 


![Controller diagram for the proposed scheme(/images/cbf_diagram1_c1.jpg)


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

