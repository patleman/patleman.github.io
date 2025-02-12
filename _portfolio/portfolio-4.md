---
title: "Augmenting Public Key Infrastructure(PKI) to PX4 source code and making it NPNT compliant( No Permission No Takeoff)"
excerpt: " This project focuses on ensuring compliance with the Government of India's regulations for drone operations by implementing software APIs for traceability, security, and operational safety. The goal is to achieve Level 0 Compliance for flight modules using Pixhawk, ensuring secure flight log signing and key management within the PX4 source code.<br/><img src='/images/npnt_2.png'>"
collection: portfolio
---
### Problem Statement
In order to establish a **secure and reliable** business environment for drones in the country, a **robust infrastructure** must be built to ensure that drone activities are **traceable**, thereby preventing misuse of the technology. To address this critical need, the Government of India has issued policies and guidelines, which can be found in the [DGCA RPAS Guidance Manual](https://patleman.github.io/files/DGCA_RPAS_Guidance_Manual.pdf). These guidelines outline essential software-level features that a drone or RPAS (Remotely Piloted Aircraft System) must incorporate to ensure safety, security, and compliance.

<img src='/images/npnt_2.png'>

To comply with government regulations for drone operations in India, the task is to implement the software APIs listed on page 51 of the [aforementioned document](https://patleman.github.io/files/DGCA_RPAS_Guidance_Manual.pdf). This will ensure that drones meet the required standards for security, traceability, and regulatory compliance in Indian airspace.

The guidelines specify two levels of compliance:

### Level 0 Compliance:
This involves implementing signing and encryption within the host system software. The private keys must be carefully managed to prevent unauthorized access. Fraudulent flight logs should not be easily injected, and the private keys must be protected from both users and external applications.

### Level 1 Compliance:
This level requires signing and encryption to be handled within a Trusted Execution Environment (TEE). The private key is fully isolated and managed within the TEE, ensuring that neither system processes nor users can access it or manipulate flight logs.

However, Pixhawk does not have a built-in TEE or secure hardware module to fully isolate private key management, which means it cannot fully meet Level 1 Compliance. Therefore, the scope of the project was to achieve **Level 0 Compliance**.



### Objective

1. Understand the software architecture of the PX4 source code and identify the modules and methods that need amendments to comply with the standards. (Controller board used: [Pixhawk](https://docs.px4.io/main/en/flight_controller/pixhawk-2.html)).
2. Select a lightweight cryptographic library to perform big number modular operations required for key pair generation and hash encryption. Further, integrate this library into the PX4 source code base.
3. Understand how XML and JSON files are signed using keys.
4. Test the validation of the Permission Artefact.
5. Test key generation inside Pixhawk.
6. Activate Return to Launch behavior upon geofence or time breach (test in simulation-in-hardware).
7. Generate and sign logs within Pixhawk (test in simulation-in-hardware).

   
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

### Learnings

1. The aforementioned modifications were directly applied to the PX4 source code (Pixhawk), which had previously been implemented on a Raspberry Pi (companion computer). This integration resulted in increased flight time and reduced power consumption for the company's RPAS.
2. Gaining an understanding of the software architecture was crucial in identifying areas that required amendments to achieve the desired behavior while keeping the main objective in mind. It became clear that reading and understanding the code often takes more time than writing it.
3. Debugging and Testing

### Tools used
PX4, PIXHAWK, GPS, MAVSDK, QGroundControl





