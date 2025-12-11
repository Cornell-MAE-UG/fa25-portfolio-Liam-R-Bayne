---
layout: project
title: Sphero Bot System Dynamics Analysis
description: MAE 3260 final group project analyzing the dynamics, ODE modeling, transfer functions, and battery modeling of the Sphero Bot
technologies: [MATLAB, System Dynamics, Control Theory]
image: /assets/images/Sphero.jpg
---

## System Overview

For our final project in **MAE 3260: System Dynamics**, my group analyzed the dynamics of the **Sphero SPRK** robot.  
We treated the Sphero as a coupled electromechanical system: a DC motor drives the internal core, which in turn produces shell rotation and translational motion through rolling without slip.

The project combined physical dissection, parameter estimation, mathematical modeling, and simulation to understand how a simple voltage command on the motor translates into real-world displacement of the robot.

You can read the full report here:  
👉 **[Download the full PDF report]({{ '/assets/pdfs/MAE3260-Spherologists-Final-Groupwork-Report.pdf' | relative_url }})**


---

## Physical Parameters and Motor Identification

During dissection, we measured:

- Shell mass, internal mass, and outer diameter  
- Geometry needed to compute moments of inertia for the internal core and outer shell  

From these measurements, we modeled the rigid bodies and converted all values into SI units for use in MATLAB.

We also identified the drive motor as a **FP130-KT/10225 6–12 V DC brushed motor**.  
Because full electrical specifications were not available, we:

- Used typical 130-size motor data to estimate armature resistance, back-EMF constant, and torque constant  
- Assumed a small inductance consistent with a millisecond-scale electrical time constant  

These parameters formed the basis for the coupled electrical–mechanical model.

---

## ODE and Transfer Function Modeling

The Sphero was modeled as four linked subsystems:

1. **Motor electrical dynamics** (voltage → current)  
2. **Internal core rotation** (current → internal angular velocity)  
3. **Shell rotation** (internal dynamics → shell angular velocity)  
4. **Shell kinematics** (angular velocity → linear displacement via rolling without slip)

Starting from Newton–Euler and standard DC motor equations, we derived a set of ordinary differential equations and converted them to the Laplace domain. From this, we obtained transfer functions that map the input voltage to:

- Motor current  
- Internal core angular rate  
- Shell angular rate and angle  
- Linear displacement of the Sphero

These transfer functions capture the full second-order dynamics of the robot and form the basis of the block-diagram representation used in the report.

---

## Simulation Results: Step and Sinusoidal Inputs

To visualize the system response, I implemented the transfer functions in MATLAB and simulated two main cases:

- A **step input** in voltage to represent a user commanding forward motion  
- A **sinusoidal input** to represent rapid back-and-forth control

### Step Response

<img src="{{ 'assets/images/sphero_step_plot.png' | relative_url }}" width="100%">

In the step case, the Sphero’s displacement grows rapidly at first and then transitions into a nearly linear increase.  
This reflects the combined effect of the motor dynamics and the integrator in the kinematic relationship between shell velocity and position. The response is appropriate for a toy robot that must respond quickly while still appearing smooth and controllable.

### Sinusoidal Input Response

<img src="{{ 'assets/images/sphero_sine_plot.png' | relative_url }}" width="100%">

With a sinusoidal voltage command, the displacement does **not** oscillate around zero. Instead, it shows an increasing phase lag and a drifting mean position. This behavior comes from the integrator in the position dynamics: any slight imbalance between positive and negative torque produces a cumulative offset in position over time.

---

## Internal Layout and Dissection

The dissection images below show the internal core, shell, and drivetrain that the model is based on.

<img src="{{ 'assets/images/sphero-expanded.png' | relative_url }}" width="100%">
<img src="{{ 'assets/images/sphero-dissection.png' | relative_url }}" width="100%">

These views guided the mass and inertia calculations and helped us interpret how the motor torque is transmitted to the shell through gears and the internal carriage.

---

## Battery Modeling as a Second-Order System

In addition to the mechanical model, our group treated the Sphero’s single-cell Li-Po battery as a **dual-RC second-order system**.  
The equivalent circuit uses:

- An ohmic resistance to capture immediate voltage drop  
- A fast RC branch to represent rapid electrochemical response  
- A slow RC branch to represent long-term diffusion effects

The resulting model behaves analogously to a mass–spring–damper system with two time scales, allowing us to predict how the terminal voltage sags and recovers when the motor current changes abruptly (such as during acceleration or braking). This provides insight into how battery dynamics affect motor performance and overall stability.

---

## My Contribution

Within the group, my work focused on the **parameter estimation and simulation**:

- Measured shell and internal masses and geometry, then converted these into usable inertia values for the model  
- Researched and identified the FP130-KT/10225 motor, then estimated electrical parameters (resistance, torque constant, back-EMF constant, and inductance) by comparison with a similar fully-specified 130-size motor  
- Implemented the derived transfer functions in MATLAB and generated the **step** and **sinusoidal** input plots shown above  
- Interpreted the simulation results, explaining phenomena such as the drifting displacement under sinusoidal drive and the rapid-then-linear growth under step input  

This role tied together the physical dissection, analytical modeling, and numerical simulation to produce a coherent view of how the Sphero responds to motor voltage commands.
