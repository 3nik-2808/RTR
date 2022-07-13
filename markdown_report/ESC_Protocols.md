# **July 07, 2022 Report: ESC Protocols**

*Presented by: Nguyễn Ngọc Nhân*

## Introduction

An ESC Protocol is the “language” that the flight controllers (FC) and ESC use to communicate, one of the most basic task is to tell how fast the motor should be spinning.

There are various kinds of protocols, however, they are divided into two main types: digital and analog.

Traditionally, PWM, an analog protocol, is widely used to communicate with drivers and ESCs, but due to the requirement of high accuracy of drones control, new protocols are also applied now to improve the performance.

## I. BLDC motors

List of contents:

1. Principle of BLDC motors.
2. Classification.

### **Principle of BLDC motors**

A brushless DC motor (known as BLDC) is a permanent magnet synchronous electric motor which is driven by direct current (DC) electricity and it accomplishes electronically controlled commutation system (commutation is the process of producing rotational torque in the motor by changing phase currents through it at appropriate times) instead of a mechanically commutation system. BLDC motors are also referred as trapezoidal permanent magnet motors.

Just like any other electric motor, a BLDC motor also consists of two main parts a stator and a rotor. Permanent magnets are mounted on the rotor of a BLDC motor, and the stator is wound for a specific number of poles. Also, a control circuit is connected to the stator winding. Most of the times, the inverter/control circuit or controller is integrated into the stator assembly. This is the basic constructional difference between a brushless motor and a typical dc motor.

Stator windings of a BLDC motor are connected to a control circuit (an integrated switching circuit or inverter circuit). The control circuit energizes proper winding at the proper time, in a pattern which rotates around the stator. Permanent magnets on the rotor try to align with the energized electromagnets of the stator, and as soon as it aligns, the next electromagnets are energized. Thus, the rotor keeps running.

For BLDC control in drone, we used an external control circuit called ESC (electronic speed controller) for high accuracy control and multiple protocols application.

### **Classification**

There are two types of BLDC motors based on their construction/design: inner rotor design and outer rotor design. Regardless of these types, note that the permanent magnets are always mounted on the rotor and winding on the stator.

- Inner rotor design:

<img src="https://1.bp.blogspot.com/-TSRi8BNJtyY/Xrwyqty4vBI/AAAAAAAADsY/5kvT2BX1hosKpa195ju1yL_FDmS1ZVRdACK4BGAsYHg/w400-h400/inrunner_bldc_motor_working.gif">

- Outer rotor design:

<img src="https://4.bp.blogspot.com/-pJATvoj-qz8/VVQtSv0RCMI/AAAAAAAABHw/abF8cTRrEck/w400-h387/bldc-working.gif">

## II. Analog Protocols

List of contents:

1. PAM, PWM and PPM.
2. PWM for ESC control and drawbacks of regular PWM in ESC control.
3. PWM technologies.
4. Other analog protocols: Oneshot and Multishot.

### **PAM, PWM and PPM**

- PAM (Pulse Amplitude Modulation): The amplitude of the pulse represents the magnitude of the signal. This is not a protocol used in ESC control (more details only).
- PPM (Pulse Position Modulation): The position of the pulse in the window represents the magnitude of the signal.
- PWM (Pulse Width Modulation): The width of the pulse in the window represents the magnitude of the signal. This width is also called “duty cycle”.

![PWM, PAM and PPM](https://media.cheggcdn.com/study/5ae/5aece0c8-d77e-43dc-879f-4b6cfd3af901/7050-9-32QP-i1.png)

### **PWM for ESC control**

ESCs usually use 50Hz (20ms period) PWM signal. This is also used in RC servo motor control.
The value of PWM duty cycle usually range from 1000μs - 2000 μs, corresponds to minimum and maximum preset value of the ESC. From there, ESC controls the speed of the BLDC motors.

![Traditional PWM for ESC](https://i.stack.imgur.com/d8WoV.png)

However, with traditional PWM that allows us to refresh the value of duty cycle every 20ms, there are some issues about with the accuracy in drone control:

- Analog protocols, especially low-width ones, can be considerably affected by electrical noises, causing inaccurate control.
- For traditional PWM, the jitter (the variation in a signal’s timing from its nominal value, this is caused by the non-unity of the ESC and the microcontroller oscillator or the dissimilar processing time between each PID loop) affects the timing for updating duty cycle value, causing delay period of 2ms for maximum, which tends to be significant in such field that requires high accuracy like drone control.
- Limited update rate (500Hz) of duty cycle value is also a problem.

![Problems with PWM](https://drive.google.com/uc?export=view&id=1Zed9Qeod4tAN8XtyQjOKNdIU1UZVCIvp)

### **PWM technologies**

To avoid jitters in traditional PWM, some of its variants are developed:

- SyncPWM: The exact moment that the PID loop outputs a new value, the flight controller communicate with the ESC to control the motors. With SyncPWM, there is no jitter exists in the process.
- FastPWM: The pulse length is shorten by 1/8, which means the resultant pulse length will be 125μs - 250μs, letting the flight controller to be able to update duty cycle value with refresh rate up to 4kHz.

### **Other analog protocols: Oneshot and Multishot**

Newer analog protocols, such as Oneshot125, Oneshot42 and Multishot all use the same principle of creation: a combination of SyncPWM and FastPWM (with even shorter pulse length).

![Combination of SyncPWM and FastPWM](https://drive.google.com/uc?export=view&id=1lPX38R4cboH0BqrQE-Cd89A-GsffVjBX)

Here below are some details of Oneshot125, Oneshot42 and Multishot:

- Oneshot125: Using mentioned FastPWM, pulse length 125μs - 250μs, maximum refresh rate of 4kHz.
- Oneshot42: Using a variant of FastPWM (3x faster than Oneshot125), pulse length 42μs - 84μs, maximum refresh rate of 12kHz.
- Multishot: Using a variant of FastPWM (10x faster than Oneshot125), pulse length 12.5μs - 25μs, maximum refresh rate of 40kHz.

Although that faster analog protocols provide us with advantages, there are also drawbacks. As mentioned, short pulse length protocols are easily influenced by electrical noises, meaning that newer analog protocols can be significantly affected.

## III. Digital Protocols

List of contents:

1. General about DShot.
2. Benefits of using DShot.
3. Structure of DShot.
4. DShot150, DShot300, DShot600, DShot1200.
5. Requirement for protocols.

### **General about DShot**

We have seen above that low refresh rate analog protocols like traditional PWM are not fast enough for drone control, and high refresh rate analog protocols such as Oneshot and Multishot are significantly affected by electrical noises (by shortening the pulse length, the output value is sent multiple times, which mean that the effect is minimized by avarage. However, there are still situation that the effect is still significant, therefore, we need new protocols that can solve the problem).

DShot is a protocol that flight controllers use for communicating with ESCs. It stands for 'Digital Shot'. It is a new digital communication protocol developed by Flyduino in collaboration with Betaflight as an alternative to Oneshot and Multishot.

### **Benefits of using DShot**

DShot provides us with these advantages:

- Error checking: a checksum allows the ESC to confirm that the data is truly what has been sent by the flight controller and there was no interference (at least to a certain degree).
- Higher resolution: in case of DShot 2000 steps of throttle resolution.
- No oscillator drift and thus no need for calibration.

### **Structrue of DShot**

Every digital protocol has a structure, also called a frame. It defines which information is at which position in the data stream. And the frame structure of DShot is pretty straight forward:

- 11 bit throttle: 2048 possible values. 0 is reserved for disarmed. 1-47 are reserved for special commands. Leaving 48 to 2047 (2000 steps) for the actual throttle value.
- 1 bit telemetry request - if this is set, telemetry data is sent back via a separate channel.
- 4 bit CRC: (Cyclic Redundancy) Check to validate data (throttle and telemetry request bit).

Therefore its 16-bit structure is:

![Frame Bits Strucure](https://www.speedgoat.com/Portals/0/adam/Content/4WNgvXMn2UG2A7Smt8d3qg/Image/Dshot.png)

The interesting part is, that 1 and 0 in the DShot frame are distinguished by their high time. This means that every bit has a certain (constant) length, and the length of the high part of the bit dictates if a 1 or 0 is being received (in DShot the high time for a 1 is always double that of a 0. The actual frame duration, bit period time and frame length depend on DShot version). This has two benefits:

- Every frame has exactly the same, easy to calculate duration: 16 x (bit period time).
- The measurement of a bit can always be triggered on a rising flank and stopped on a falling flank (or the other way around in case of the inverted signal with bidirectional DShot).

<img src="https://brushlesswhoop.com/images/dshot_bit.png"  width=60% height=60%></img>

### **DShot150, DShot300, DShot600, DShot1200**

The actual frame duration, bit period time and frame length depend on DShot version:

|**DShot**|**Bitrate**|**T1H**|**T0H**|**Bit (µs)**|**Frame (µs)**|**Possible FC loop frequency**|**Recommended FC loop frequency**|
|---|---|---|---|---|---|---|---|
|150|150kbit/s|5.0|2.5|6.67|106.72|8.33 kHz (2-bit reset)|4.05 kHz (21-bit reset time)|
|300|300kbit/s|2.50|1.25|3.33|53.28|16.6 kHz (2-bit reset)|8.09 kHz (21-bit reset time)|
|600|600kbit/s|1.25|0.625|1.67|26.72|33 kHz (2-bit reset)|16 kHz (21-bit reset time)|
|1200|1200kbit/s|0.625|0.313|0.83|13.28|66 kHz (2-bit reset)|32 kHz (21-bit reset time)|

- T1H is the duration in µs for which the signal needs to be high in order to be counted as a 1.
- T0H is the duration in µs for which the signal needs to be high in order to be counted as a 0.

With DShot, we can reach high update rate without being affected by electrical noises. However everything has two sides, and so do digital protocols. The downsides are, that the digital protocols are not the fastest since they carry an overhead like - in the case of DShot - the CRC, which adds reliability but also increases the duration of a frame and thus data needed to be transmitted. Also frames are always of a fixed length, no matter if you are going full throttle or no throttle - wheres the length of a pulse is shorter with analog when the throttle value is smaller.

### **Requirement**

To perform BLDC control with a specific protocol, we need the cooperation of three parts: flight controller, firmware and ESC.

- Flight controller: Not every flight controller supports all type of ESC protocols, therefore we have to be careful when choosing the flight controller.
- Firmware: The firmware must support the desired protocols also.
- ESC: Analog protocols are supported by nearly all type of recent ESCs, but not for digital protocols. Because of that, we also need choose the right ESC that fits our purposes.
