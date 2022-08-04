# July 28, 2022 Report: Dynamic Configuration

*Presented by: Nguyễn Ngọc Nhân*

## Introduction

There are various components that contribute to the dynamic aspect of the quadcopter, including power supply, actuators and more. Each of this components can affect the motion of the quadcopter in different ways. Therefore, to maintain the stability of quadcopter control, a research to determine the optimal (specifically optimal to given purpose/mission) dynamic configuration is essential.

## I. Dynamic Components

List of contents:

1. General about dynamic components.
2. Frame.
3. BLDC Motors.
4. Propellers.
5. Batteries.

### **General about dynamic components**

As indicated, there are various components that helps the quadcopter to be able to move, however, in general, they include:

- Frame: The quadcopter's frame can be considered as its main body. Motors, ESCs, battery, external sensors and other devices are attached to the frame.
- Motors: Play the role of the actuators of the quadcopter. Not only providing thrust via propellers, they also control its direction and postures.
- Propellers: The thrust generated not only depends on the motors but also the propellers (shape and size) as well.
- Batteries: The power supply for the quadcopter. Aside from battery's capacity, its rate of power supplying also matters.

These components are the fundamental parts of dynamic aspect of the quadcopters. The parameters need noticing are demonstrated below in each specific section corresponding to each component.

### **Frame**

There are countless types of quuadcopters's frames, therefore, to be able to choose the suitable one, we will consider them in three aspects: material, size and shape.

About the material, the frame of an quadcopter can be made of various kinds of material, such as wood, plastic, aluminium, etc. However, recently, carbon fiber is most favoured due to the combination of its high durability and light weight. Most of FPV quadcopters' frames these days are made of carbon fiber.

<img src="https://scitechdaily.com/images/Carbon-Fiber-Illustration.jpg" width=500>

The size of the frame is significantly affected by the quadcopter's mission, to be specific, the weight carried. The total mass is proportional to the value of thrust, meaning that the mass also affects the size of propellers, resulting in the changes in size of the frame. Generally, the frame size is determined by the distance from opposite corner motors.

<img src="https://oscarliang.com/ctt/uploads/2017/03/mini-quad-frame-diagonal-motor-to-motor-distance-wheelbase-measurement.jpg" width=500>

The shape of the frame is also highly dependent on quadcopter's purpose. For example, a quadcopter used for racing (usually FPV) has the frame with the design of optimal aerodynamics. In contrast, with ones used for investigation/shipping, stability and high accuracy control are more important than speed, therefore the frame will be designed in a way that makes the quadcopter moves stably. Besides aerodynamics, the modules attached to the frame also affect its structure. With the number and size of modules vary, the frame also needs suitable design.

<img src="https://oscarliang.com/ctt/uploads/2017/03/types-style-h-x-hybrid-X-box-mini-quad-frame.jpg" width=500>

Below are some arm layout demonstrated in [OscarLiang](https://oscarliang.com/fpv-drone-frames/) and their usage:

- H Frames: On an “H” frame, the arms sticks out from the sides of the body at the front and rear, it looks like the letter “H”. This leads to a long and roomy body section that for installing your electronics comfortably, makes it very easy to build. Most older frames used to be H frames and very bulky. HD camera and battery can both sit on the top plate, the weight of the quad is more spread-out. For this reason, it might feel less agile than the newer X frames due to higher moment of inertia on the pitch axis.

<img src="https://d2t1xqejof9utc.cloudfront.net/screenshots/pics/9dd3a4062e9420afd1c4f98f365e8d29/large.jpg" width=500>

- True-X Frames: On a “True-X” frame all the arms meet at the centre forming a shape that looks exactly like an “X”, the distance between all the motors are equal. True-X frames tend to have a more balanced performance as the motors are contributing equally. Some argue it has little to no difference to an H frame in terms of flight characteristics, since the mass distribution is the same, while others argue there is a difference to how the thrusts are applied on the frame due to the way the arms are connected to the body (think about leverage).  Also the ways vibrations are transferred to the FC through the arms are different too.

- Stretch X Frames: Similar to a “true-X” frame in appearance, the “stretch X” frame moves the front and rear motors further apart from each other. The idea is to reduce air disturbance that the rear propellers get from the front propellers, and it could improve high speed handling especially in corners.

<img src="https://oscarliang.com/ctt/uploads/2017/03/stretch-x-true-x-mini-quad-frame.jpg" width=500>

- Deadcat Frames: Often used in cinematic rigs (for cruising and capturing cinematic footage using a GoPro camera) as the front motors are wider apart than the rear motors, the propellers are normally not visible in the camera’s view and doesn’t ruin the beautiful shot.

<img src="https://imgaz1.staticbg.com/thumb/large/oaupload/banggood/images/11/2D/4ff81aef-159c-43a7-b525-b493643bea98.jpg" width=500>

- Square Frames: A square frame, or box frame, is pretty much an enclosed X Frame. This basically creates a tougher frame that is less likely to have broken arms. However the extra material creates more drag and weight. Not the best choice for performance but definitely helps improve strength of the arms.

- Plus Frame: Instead of flying forward like an “X”, a Plus Frame flies forward like a “+” sign. There may be some benefits with regards to motor turbulence though, because the side motors on a plus frame will always be spinning in clean air. One of the downsides is the front motor and propeller can easily get in the view of the camera. It’s not common frame type apart from the fact that it looks unique and is eye-catching.

<img src="https://oscarliang.com/ctt/uploads/2017/03/mini-quad-frame-diatone-plus-configuration-racing-drone.jpg" width=500>

### **BLDC Motors**

The motors are the actuators of the quadcopter. In terms of dynamics, they provide thrust via rotational motion of the propellers, lifting the quadcopter up from the ground. Therefore, the motors need to be chosen carefully so that it can adapt our demands. In the market, there are many types of motors with different technical parameters. Below are fundamental specifications which need to be noticed in the datasheet of BLDC motors:

- Nominal voltage: The highest electric potential where the BLDC motor can work continuously.
- Nominal torque: The maximum torque where the BLDC can work continuously.
- Nominal current: The maximum current where the BLDC can work continuously.
- Stall torque: The maximum torque where the BLDC can handle in a short period of time.
- Stall current: The maximum current where the BLDC can handle in a short period of time.
- Speed constant (Kv): The nominal RPM with each volt provided at unloaded condition.
- Torque constant (Km): The nominal torque with each ampere provided.

### **Propellers**

In constrast of fixed-wing aircrafts, quadcopters use the principle of propellers as the fundamental to lift the quadcopter from the ground. The propellers of a quadcopter are used to transform the rotational power of the motors into thrust. The propeller works by displacing the air pulling it below itself, this movement of air then results in the quadcopter being pushed up from the resulting pressure difference. The more air that is pulled below the propeller the more thrust or upward propulsion is generated.

<img src="https://www.darcorp.com/wp-content/uploads/2018/11/Propeller1.gif" width=500>

The shape of the propellers, however, is complicated in terms of aerodynamics. Therefore, we will be choosing the size of the pre-designed propellers only. Simply speaking, the heavier the weight carried is, the larger the thrust required, resulting in the bigger propellers and stronger BLDC motors.

### **Batteries**

Batteries play the role of the main power supply for the quadcopter. The batteries need to satisfy these condition:

- Its storage of energy must be large enough to adapt the mission.
- The flow of energy must be enough for the quadcopters' motors at the maximum speed.
- The batteries' electric potential should fit the motors.

The quantities that correspond to first two conditions are batteries' capacity and discharge rate. The capacity of the batteries stand for the electrical energy stored in the battery, usually measured in mAh (miliAmpere-hours) and the discharge rate stand for the maximum current that the battery can provide, usually measured in C (to compute the current, we take the capacity multiply with the discharge rate).

## II. Components Selection

List of contents:

1. Weight Calculation.
2. Thrust and Power Calculation.
3. Propellers Selection.
4. Motors Selection.
5. Battery Selection.
6. Frame Selection.

### **Weight Calculation**

To start bulding a quadcopter, first, its mission needs to be determined. Then, based on the mission, the estimated weight of the quadcopter will be computed. The total weight consists of FC, frame (estimated), ESCs, battery (estimated), battery adapter, motors (estimated), props (estimated). Then, the total weight will be divided by 4 corresponding to 4 motors, resulting in the thrust needed to hover.

### **Thrust and Power Calculation**

There is an efficiency coefficient between the power input to the motors and the output power (actual power) that generated by the propellers. Therefore, to compute the essential power for the quadcopter, we need to consider that coefficient in order to estimate the size of propellers.

### **Propellers Selection**

Despite the similarity in size, each types of propeller has different specifications, including coefficient, maximum rotational speed endured (limited RPM), etc. To choose the propellers properly, we have to consider these specifications.

- The power coefficient, or power factor of the propeller, depends significantly on its shape. This factor is usually written in the datasheet of each type. The ratio between the input and output power is called figure of merit (FoM).
- Limited RPM highly depends on propeller's size and shape. The limited RPM is usually computed as constant (depend on shape) divided by diameter (inch).

Usually, we will choose the propeller with minimum size which is still able to adapt our purpose and maximum FoM to obtain the optimization in terms of power and mass.

### **Motors Selection**

After selecting the propellers, we then choose motors to fit those. Besides indicated specifications, we have to consider the effiency of the motor in the working range. Unfortunately, not every motor comes with graphs or thrust table (providing information about thrust generated (types of propeller would be mentioned) as full throttle and current used at that state) as below, therefore, sometimes it is necessary to execute some experiment in order to get the data of the motors.

<img src="https://uav.jreyn.net/user/pages/04.quadcopter-design/step-5-motor-selection/BL16B17-08_NEW_HR.jpg" width=500>

<img src="https://uav.jreyn.net/user/pages/04.quadcopter-design/step-5-motor-selection/20131024034814598.jpg" width=500>

With the propellers chosen and required thrust computed, we can choose the motors for quadcopter base on thrust table like this.

### **Battery Selection**

As mentioned, when selecting batteries, the capacity, discharge rate and electric potential must be considered. Besides, batteries' mass also should be taken into account. With the same current and electric potential, the larger the capacity, the larger the mass. Therefore, to optimize the time flight, the battery should be chosen carefully.

### **Frame Selection**

The frame is what we choose at the end of the components selection stage. To get the suitable frame for our quadcopter, these conditions should be satisfied:

- There must be space for the propellers to be placed seperately (size selection).
- Battery's position must be prepared in order to calculate the center of mass (main body space selection).
- Motors installation method must be prepared.
- The frame should fit the quadcopter purposes.

## Reference

- [Quadcopter Design](https://uav.jreyn.net/quadcopter-design)
