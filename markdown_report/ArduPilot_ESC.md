# **June 14, 2022 Report: ESC control in ArduPilot**

*Presented by: Nguyễn Ngọc Nhân*

## Introduction

To control a drone, its flight controller must command the ESCs to control the motors in an appropriate way. Fortunately, over years, autopilot systems have been developed and some of them are open-sourced, which allows us to program our drone with ease. One of them is ArduPilot, a trusted, versatile, and open source autopilot system supporting many vehicle types: multi-copters, traditional helicopters, fixed wing aircraft, boats, submarines, rovers and more.

## I. ArduPilot Installation for Windows

List of contents:

1. Components for ArduPilot on Windows.

2. Installation Procedure.

### **Components for ArduPilot on Windows**

To be able to run ArduPilot on Windows, we need following components:

- ArduPilot's main program (by achieving a GitHub clone of ArduPilot project). The link to Ardupilot official repository is here: [ArduPilot's GitHub](https://github.com/ArduPilot/ardupilot).

- Windows Subsystem for Linux - WSL (ArduPilot runs on Unix-like OS, therefore, if we want to run ArduPilot on Windows, we need a simulation of Unix-like OS).

<img src="https://codewithbish.com/wp-content/uploads/2021/06/codewithbish-Guide-to-install-Windows-Subsystem-for-Linux-WSL-2-on-windows-10.png">

- A Unix-like OS (Ubuntu is recommended in the documentation).

- XWindows application to run graphical programs.

- Flight simulating softwares such as Mission Planner or X-Plane.

### **Installation Procedure**

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/ArduPilot_logo.svg/2560px-ArduPilot_logo.svg.png" width="900">

To install ArduPilot on Windows, we need to follow the procedure (the detailed procedure is available on ArduPilot documentation):

1. WSL and Ubuntu must be installed and tested first before ArduPilot installation.

2. Initialize Ubuntu (create username and password), then accessing it.

3. Install Git in Ubuntu.

4. Clone ArduPilot's repository and install required packages, then reload the path.

5. Install an XWindows application to run graphical programs such as SITL by installing VcXsrv, Cygwin X or Xming on Windows.

6. Install a flight simulating software such as Mission Planner or X-Plane.

## II. ESC Control in ArduPilot

List of contents:

1. Communicating Protocols.

2. Hardware and firmware supporting DShot.

3. ESC calibration methods.

### **Communication Protocols**

Beside signal protocols like PWM, Oneshot, Multishot or DShot, communication protocols (which maintain appropriate connection) are also significant. Not only providing us with appropriate connection, communication protocols also bring us various functionalities. Below are some popular protocols used in drone control (all supported by ArduPilot):

- DroneCAN: The primary CAN protocol used by the ArduPilot and PX4 projects for communication with CAN peripherals. It is an open protocol with open communication, specification and multiple open implementations.

- FETtec OneWire: OneWire is an ESC bi-directional communication protocol from FETtec. FETtec OneWire ESC’s telemetry information is sent back to the autopilot:

    * Motor revolutions per minute (RPM).
    * Input voltage (V).
    * Current draw (A).
    * Power consumption (W).
    * Temperature (°C).
    * Summed CRC errors.

- KDECAN: Allow UVC series ESCs (electronic speed controllers) to actively monitor critical system components, increase redundancy, and improve operational safety for drones.

### **Hardware and firmware supporting DShot**

As we know it, to use DShot for ESCs communication, there must be cooperation between hardware and firmware, meaning that both hardware and firmware must support DShot. First, to choose the correct hardware for DShot protocols, there are two main parts to consider: FCs and ESCs:

- About FCs, the official GitHub of Betaflight contains the details (including calculation and setup instruction) of DShot ESC protocols and which FCs have been tested for the ability to run DShot. [Here](https://github.com/betaflight/betaflight/wiki/DSHOT%20ESC%20Protocol#flight-controllers-tested-to-support-dshot-on-betaflight-31-without-mods-or-remapping) is the list of FCs that have been verified for the availability of DShot using Betaflight 3.1.

- About ESCs, there are two main groups that support DShot: BLHeli_S ESCs and KISS ESCs.

    * BLHeli_S ESCs: In Nov 2016, certain ESC’s that run BLHeli_S firmware would support DShot, such as Cicada, Racerstar V2, Aikon SEFM, TBS 25A, Lumenier 30A, DYS XS30A, etc. However, these ESC’s have speed restriction and only support up to DShot300, some can only run DShot150. In 2017, there was an update states that most BLHeli_S ESC’s that just came out of the market would support DShot out of the box, but some older ones might require you to remove a capacitor in order to run D-shot. This capacitor is designed to filter out noise at the input, but because DShot is so high frequency it will get filtered too.
    * KISS ESC: KISS 24A ESC supports DShot with 3 speed levels (DShot150, DShot300 and DShot600), available to choose from in their Chrome GUI configurator. KISS 24A ESC’s use STM32 processors, our mini quad FC’s also use STM32 MCU‘s. Between these STM32 chips they have something called DMA controller (Direct Memory Access) that communicate directly between them. This allows faster DShot signal as it doesn’t use much additional processing resource that might cause delay.

## Reference

1. ArduPilot Installation: [ArduPilot's Official Documentation, "Building the code" part](https://ardupilot.org/dev/docs/building-the-code.html).

2. Communicating Protocols: [ArduPilot's Official Documentation, "Peripheral Hardware" part](https://ardupilot.org/copter/docs/common-escs-and-motors.html).

3. Hardware and Software supporting DShot: [Information about DShot, BLHeli_S and KISS ESCs](https://oscarliang.com/dshot/).
