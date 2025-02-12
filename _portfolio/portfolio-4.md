---
title: "Augmenting Public Key Infrastructure(PKI) to PX4 source code and making it NPNT compliant( No Permission No Takeoff)"
excerpt: " This project introduces a hierarchical Control Barrier Function (CBF)-based control framework designed to proactively ensure the safe operation of an industrial manipulator in close human-robot interaction scenarios."
collection: portfolio
---
### Problem Statement
In order to establish a **secure and reliable** business environment for drones in the country, a **robust infrastructure** must be built to ensure that drone activities are **traceable**, thereby preventing misuse of the technology. To address this critical need, the Government of India has issued policies and guidelines, which can be found in the [DGCA RPAS Guidance Manual](https://patleman.github.io/files/DGCA_RPAS_Guidance_Manual.pdf). These guidelines outline essential software-level features that a drone or RPAS (Remotely Piloted Aircraft System) must incorporate to ensure safety, security, and compliance.

<img src='/images/npnt_2.png'>

In alignment with these regulations, the task is to implement the **software APIs** listed on page 51 of the [aforementioned document](https://patleman.github.io/files/DGCA_RPAS_Guidance_Manual.pdf), ensuring that drones meet the required standards for operation in the Indian airspace. The goal is to develop a system that adheres to these regulatory requirements and guarantees the traceability and security of drone operations for regulatory and safety purposes.
The governement guidelines list particularly two levels of compliance: Level 0 Compliance for Flight Module security ensures that signing and encryption are implemented at the software level within the host system, with careful management of private keys to prevent unauthorized access. It requires that private keys are protected from users or external applications, and fraudulent flight logs cannot be easily injected. Level 1 Compliance elevates security by implementing signing and encryption within a Trusted Execution Environment (TEE), ensuring that neither host system processes nor users can access the private key or manipulate flight logs. In Level 1, private key management is fully contained within the TEE. Pixhawk does not have a built-in TEE or similar secure hardware module that can fully isolate private key management and prevent access by host system processes or external applications.


### Objective

1. Understand the software architecture of PX4 source code and figure out the modules, methods that needs amendements to be compliant with the 
 standards. (controller board used:[Pixhawk](https://docs.px4.io/main/en/flight_controller/pixhawk-2.html))
2. Select a lightweight cryptographic library to run big num mod operations used in generation of key-pair and encrypting hashes. Further, integrate this to the Px4 source code base.
3. know how XML files and JSON files are signed using keys.
4. Test the Validation of Permission Artefact
5. Test Key Generation inside Pixhawk 
6. Return To Launch behavior is activated upon geofence or time beach. (tested in simulation-in-hardware)
4. Log is generated and signed within the Pixhawk(tested in simulation-in-hardware)
   
### Method 
<img src='/images/chart_npnt.png'>

### Challenges faced
Problems faced in Software-in-the-Loop (SITL):

1) **Canonicalization and validation of permission artifacts (XML file)**: The XML parser library could not be used due to **linking issues**.
2) **Canonicalization and signing of flight logs (JSON file)**: The approach for canonicalization and signing of flight logs was not straightforward, leading to difficulties in implementation.

---

Problems faced while implementing the code in hardware:

1) **Memory and Stack Space**: 
    Processes like **permission artifact validation**, **flight log signing**, and **key generation** (involving operations like **random prime number generation** (1024-bit long) and **modular exponentiation**) require significant computation and use **huge stack space**. The **inbuilt memory** of Pixhawk is around **256 KB**, most of which is already consumed by the existing tasks in the **PX4 firmware**. Because of this, when the code was uploaded to the Pixhawk, it didn't respond due to **full stack usage during computation**. 
   
   To address this, it was found that **at pre-boot time**, most of the stack space is available, as other tasks are not running. Therefore, all the      heavy computational processes were **moved to the pre-boot time** for successful execution.

2) **Key Generation and Random Number Generation**: 
   For **key generation** inside the Pixhawk, **random number generation** is essential. Initially, this was not achievable. Later, it was found that some **device drivers** responsible for **random number generation** were not registered in the initial configuration of **NuttX-RTOS**. The **NuttX RTOS** was then reconfigured to include **/dev/random** and **/dev/urandom** (devices responsible for generating random numbers), solving the issue.

3) **Memory Limitations in SITL vs. Hardware**: 
   The code tested in **Software-in-the-Loop (SITL)** used too much memory, which caused the same code to perform poorly when running on hardware. Both the **stack overflow** issue mentioned in point 1 and the **huge size arrays** defined in the code contributed to this problem. To resolve this, the **code architecture** was redefined for the hardware simulation to achieve the same behavior without overloading the memory.







