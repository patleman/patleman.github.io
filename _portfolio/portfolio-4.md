---
title: "Augmenting Public Key Infrastructure(PKI) to PX4 source code and making it NPNT compliant( No Permission No Takeoff)"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios.<br/><img src='/images/cbf_diagram1_c1.jpg'>"
collection: portfolio
---


The overview of the developed algorithm is shown in the diagram below: 
<img src='/images/CBFcontroller (1).png'>

The equations used in the algorithm is shown in the diagram below:
<img src='/images/equation_controller_po.drawio.png'>



1) Validation of Permission Artefact is tested
2) Key Generation inside Pixhawk is tested
3) Return To Launch behavior is activated upon geofence or time beach. (tested in simulation-in-hardware)
4)Log is generated and signed within the Pixhawk(tested in simulation-in-hardware)

Problems faced in Software in the Loop:
1) knowing and implementing the canonicalization and validation of permission artifacts (XML file). XML parser library was not able to be used because of linking issues.
2) way of canonicalization and signing flight log (JSON file).

Problems faced while implementing the code in hardware:

1)Processes like permission artifact validation, flight log signing, and key generation involve computations like random prime number generation(1024 bit long in this case) and modular exponentiation. These computations use huge stack space. The inbuilt memory is around 256-kilobytes.  Most of which is already taken up by the already coded tasks in px4 firmware. Because of the just mentioned facts, when I earlier uploaded the code to the PIXHAWK, it didn't respond (because of full stack usage during computation).
At the pre-boot time, most of the stack space is available as the other tasks are not running at that time. Therefore later,  all the heavy computation processes were compelled to run at the pre-boot time.

2) For generating keys inside PIXHAWK, the generation of random numbers is required. Earlier, this was not able to be achieved. Later on, it was found that some device drivers (responsible for random number generation )were not registered in the initial configuration of the NUTTX-RTOS. The NUTTX RTOS was later on reconfigured with /dev/random and /dev/urandom (responsible for generating random numbers).

3)Earlier tested code in Software-in -the loop(SITL) used too much memory. Due to which the same code didn't run well in the simulation in hardware. Both the problem mentioned in point 1 and the huge size arrays defined were the source of this problem. The code architecture earlier coded for SITL was redefined for achieving the same behavior in simulation-in-hardware.



