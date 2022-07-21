# **July 21, 2022 Report: Sensors**

*Presented by: Nguyễn Ngọc Nhân*

## Introduction

To manipulate drone in an appropriate manner, multiple sensors must be used to evaluate surroundings condition and drone's current status, such as tilt angle, acceleration, velocity, altitude, position (local/global position), temperature, wind speed, etc. In this report, sensors that are generally used in drone will be demonstrated.

## I. Drone's sensor ecosystem

List of contents:

1. Compulsory and optional input.
2. Corresponding sensors.

### **Compulsory and optional input**

Fundamentally, to manually control a drone (with local direction and velocity), the respond of tilt angles, acceleration and angular velocity are essential so that the drone is able to move around with desired direction and velocity (which can be verified by evaluation from measure acceleration in seperated axes). Altitude respond is also necessary for height maintaining (hovering) and landing.

Optionally, with autopilot additional functionalities, there need to be a method to determine the drone posture and location, in other word, it requires real-time positional and gestural data. There are various ways to achieve this, depend on the designed working environment of the drone (indoor or outdoor).

Additionally, for specific purposes, drones can be equipped with sensors for measuring or collecting desired data.

### **Corresponding sensors**

For the fundamental parameters, these sensors need to be install (usually integrated to the FC):

- Accelerometer: Measuring acceleration in 3 local axes, the accelerations are then used to evaluate velocity and tilt angles.
- Gyroscope/Gyrometer: Measuring angular velocity about 3 axes, these parameters help to adjust the local direction of the drone and verify the rate of change in tilt angles.
- Barometer: Measuring the atmosphere pressure at that position. This data helps to derive the height of the drone.

![MPU6050 - Accelerometer and Gyroscope](https://product.hstatic.net/1000069225/product/upload_3e6c3d7abd24438397162522df246cff_large.jpg)

<img src="https://nshopvn.com/wp-content/uploads/2019/03/cam-bien-ap-suat-khong-khi-bmp180-qesm-1-scaled.jpg" width = 480>

With autopilot function included, we have:

- GPS: For outdoor locating, giving global coordination to the FC.
- Antenna/Receiver: For indoor locating, giving relative coordination to the FC.
- Compass: Determine drone direction using Earth's magnetic field.

<img src="https://nshopvn.com/wp-content/uploads/2019/03/mach-dinh-vi-gps-gy-neo-6m-v2-Y180-2020-600x600.jpg" width = 500>

<img src="https://cf.shopee.vn/file/e30fce41009c0863fdc610774c7d5c59" width = 500>

Additional sensors that can be installed for special purposes:

- IR Camera: Provide a heat vision, which can be used for human detection in military/rescue purposes.

- LIDAR: Responding terrain, which helps in geography investigation or military purposes.

<img src="https://www.smart-prototyping.com/image/cache/data/2_components/Raspberry%20Pi/101776%20Raspberry%20Pi%20Infrared%20Camera%20Module/raspberry-pi-infrared-camera-module-a61604-750x750.jpg" width=400>

<img src="https://ae01.alicdn.com/kf/HTB1GAcIcA9E3KVjSZFGq6A19XXaP/ch-nh-x-c-cao-1-chi-u-Lidar-Thi-t-B-T-m-T.jpg_Q90.jpg_.webp" width=400>

## II. Details about sensors

List of contents:

1. Accelerometer.
2. Gyroscope/Gyrometer.
3. Barometer.
4. Compass.
5. GPS.

### **Accelerometer**

As mentioned, accelerometer is a device that return acceleration measured in its 3 local axes x, y and z. To accomplish this, it uses a structure that works like a capacitors. Base on the plate movement, the corresponding value of capacity will be calculated, from there, the acceleration in a local axis is determined. To measure acceleration in 3 axes, advanced structures of the indicated model are applied.

![Accelerometer's Structure](https://www.electronicwings.com/storage/PlatformSection/TopicContent/128/description/1_Accelerometer_Sensor_MEMs_Mechanism.PNG)

There are some configurations that need to be done to calculate the accurate acceleration:

- Low-pass filter: Usually, there is an analog low-pass filter integrated in the sensors. To avoid the effect of magnetic field of BLDC motors, this filter need to be set with a specific cutoff frequency.

- Full-scale range: The full-scale range needs to be set to fit the working condition and optimize calculating speed.

After obtaining acceleration in 3 axes, the tilt angles can be computed as:

$\theta = arctan\left(\dfrac{A_X}{\sqrt{(A_Y)^2 + (A_Z)^2}}\right)$

$\phi = arctan\left(\dfrac{A_Y}{\sqrt{(A_X)^2 + (A_Z)^2}}\right)$

The $\psi$ angle (around z-axis) is computed by integrating angular velocity about z-axis with initial condition at $0\degree$.

### **Gyroscope/Gyrometer**

The gyroscope returns angular velocity value $\omega$ in three axes x, y and z. There are also some configuration need to be done to compute the accurate angular velocity:

- Sample rate: This is set based on the demands, with low-pass filter enabled, the sample rate can be lowered.

- Low-pass filter: As mentioned, there can be noises from BLDC motors, therefore the low-pass filter need to be enabled.

- Full-scale range: Similar to accelerometer, this value is set based on specific purpose/mission.

To compute the $\psi$ angle, we take the integration of $\omega_Z$: $\psi = \int_0^t{\omega_Zdt}$.

There is a popular module named MPU6050 that includes accelerometer and gyroscope. Here is it [datasheet](https://www.alldatasheet.com/view.jsp?Searchword=Mpu-6050%20datasheet&gclid=Cj0KCQjwz96WBhC8ARIsAATR253gdo36r895V4gVSMdLs6_dpdllVI2ga1SWt2u7Qt_EbcH-lqVtVx4aAsC1EALw_wcB) and [register map](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Register-Map1.pdf).

![MPU6050](https://product.hstatic.net/1000069225/product/upload_3e6c3d7abd24438397162522df246cff_large.jpg)

### **Barometer**

The barometer return a value of atmospheric pressure at it position. It has been known that atmospheric pressure and height is directly related, therefore we can determine the drone's height using barometer.

The absolute altitude (with respect to the sea level) formula provided by [BMP180's datasheet](https://pdf1.alldatasheet.com/datasheet-pdf/view/1132068/BOSCH/BMP180.html):

$h = 44330 \times \left[ 1 - \left(\dfrac{p}{p_0}\right)^{\dfrac{1}{5.255}} \right]$

### **Compass**

To determine global direction and posture of the drone, a compass sensor needs to be included. This compass sensors return magnetic field value in 3 axes x, y and z. These values can also be used to verified $\psi$ angle.

<img src="https://ae01.alicdn.com/kf/H0e785cb92e0e4090a8fc0fa8798a4ce03/Diymore-GY-273-HMC5883L-3-B-Ba-Tr-c-La-B-n-T-K-C-m.jpg_Q90.jpg_.webp" width = 400>

### **GPS**

For autopilot, the location is usually determined by GPS (Global Positioning System). The distance from at least 4 satelites to the sensors would be measured, from there, the longitude, latitude and altitude can be found. To increase the accuracy, the number of satelites communicated is increased so that the errors are minimized, resulting in better performance of the drone.

## III. FC integrated sensors

List of contents

1. Cube Orange FC.
2. Cube's integrated sensors.

### **Cube Orange FC**

The information below is from the [PX4 Official Documentation](https://docs.px4.io/v1.12/en/flight_controller/cubepilot_cube_orange.html).

![Cube Orange FC](https://docs.px4.io/v1.12/assets/img/cube_orange_hero.944ff463.jpg)

The Cube Orange flight controller is a flexible autopilot intended primarily for manufacturers of commercial systems.

The controller is designed to be used with a domain-specific carrier board in order to reduce the wiring, improve reliability, and ease of assembly. For example, a carrier board for a commercial inspection vehicle might include connections for a companion computer, while a carrier board for a racer could includes ESCs for the frame of the vehicle.

Cube's key features:

- 32bit STM32H743ZI (32bit ARM Cortex M7, 400 MHz, Flash 2MB, RAM 1MB).
- 32 bit STM32F103 failsafe co-processor
14 PWM / Servo outputs (8 with failsafe and manual override, 6 auxiliary, high-power compatible).
- Abundant connectivity options for additional peripherals (UART, I2C, CAN).
- Integrated backup system for in-flight recovery and manual override with dedicated processor and stand-alone power supply (fixed-wing use).
- Backup system integrates mixing, providing consistent autopilot and manual override mixing modes (fixed wing use).
- Redundant power supply inputs and automatic failover.
- External safety switch.
- Multicolor LED main visual indicator.
- High-power, multi-tone piezo audio indicator.
- MicroSD card for high-rate logging over extended periods of time.

### **Cube's integrated sensors**

Cube Orange uses SPI protocol to communicate with integrated sensors, including:

- Accelerometer: (3) ICM20948, ICM20649, ICM20602.
- Gyroscope: (3) ICM20948, ICM20649, ICM20602.
- Compass: (1) ICM20948.
- Barometric Pressure Sensor: (2) MS5611.
