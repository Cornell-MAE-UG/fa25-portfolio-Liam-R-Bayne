---
layout: project
title: Torque Wrench Design & FEM Analysis
description: Instrumented torque wrench with CAD modeling, hand calculations, and FEM validation
technologies: [MATLAB, ANSYS, Fusion 360]
image: /assets/images/Wrench-Image.webp
---

## CAD Model

Below are the two CAD images used for the design phase: 
- A full view of the wrench with overall dimensions 
- A zoomed-in view of the drive region showing detailed geometry

<img src="{{ 'assets/images/Wrench Dimensions.jpg' | relative_url }}" width="100%">
<img src="{{ 'assets/images/Drive-Dimensions.jpg' | relative_url }}" width="100%">

---

## Material Selection: 2014-T651 Aluminum Alloy

For this design, I used **2014-T651**, a high-strength aluminum alloy (aluminum with a small percentage of titanium and copper). 
It offers a good balance of stiffness, fracture resistance, and fatigue performance, making it suitable for tool applications such as torque wrenches.

The mechanical properties used in all hand calculations and FEM verification were:

| Property | Symbol | Value |
|----------|--------|--------|
| Young’s Modulus | E | 10.5 × 10⁶ psi |
| Poisson’s Ratio | ν | 0.33 |
| Allowable Stress | σ_allow | 60,000 psi |
| Fracture Toughness | K_IC | 22,000 psi·√in |
| Fatigue Strength @ 10⁶ cycles | σ_fatigue | 20,000 psi |

These values were used to evaluate the required safety factors:

- **Strength safety factor** ≥ 4 
- **Fracture toughness safety factor** ≥ 2 
- **Fatigue safety factor** ≥ 1.5 

2014-T651 meets all three requirements for the chosen geometry when evaluated with both hand calculations and FEM results.

---

## FEM Boundary Conditions and Loading

To validate the hand-calculated design, I created a finite element model of the torque wrench in ANSYS. 
The following image shows the applied loads and boundary conditions used in the simulation:

<img src="{{ 'assets/images/Loads-BC.png' | relative_url }}" width="100%">
<img src="{{ 'assets/images/Loads-BC-2.png' | relative_url }}" width="100%">

### Boundary Conditions

- The **top faces of the drive** were fully constrained (ux = uy = uz = 0) to represent how the square drive engages with a rigid socket.
- This prevents all translational motion at the drive end while allowing the handle to bend under load.

### Applied Load

To apply the correct bending moment:

- The required moment of **600 in-lbf** was converted into an equivalent force: 
\[
F = M/L
\]
- This force was applied at the **load point** (the tip of the handle), located a known distance from the drive.

This method ensures that ANSYS experiences the same bending moment distribution used in hand calculations, enabling a direct comparison of stresses, strains, and deflection.

---

## Strain Contour in the Gauge Direction

To confirm that the gauge placement captures sufficient strain for torque measurement, I generated a strain contour plot in ANSYS showing the normal strain along the gauge axis. This corresponds to the bending strain measured by the strain gauges bonded to the top and bottom surfaces of the handle.

<img src="{{ 'assets/images/Strain Contours.png' | relative_url }}" width="100%">

### Interpretation

- The strain field is largest near the fixed drive and decreases toward the free end, as expected for a cantilever under bending.
- The gauge location (1 inch from the drive) sits in a region of **smooth, uniaxial bending strain**, ensuring consistent readings.
- The strain at the gauge extracted from this contour was later used to compute the torque wrench sensitivity using the relationship:

**mV/V output ≈ (gage factor) × (strain at gauge) × (bridge factor)**

where the bridge factor is **1/2** for a half-bridge configuration.

This contour verifies that the gauge region experiences measurable and uniform bending strain without distortion from local stress concentrations.

---

## Maximum Principal Stress Contour

To assess the structural safety of the torque wrench under the full 600 in-lbf load, I generated the maximum principal stress contour in ANSYS. Principal stresses provide a clear measure of the highest tensile stress anywhere in the model, which directly determines the safety factors for yielding, fracture, and fatigue.

<img src="{{ 'assets/images/Contour Stress.png' | relative_url }}" width="100%">

### Interpretation

- The highest stresses occur near the **fixed drive region**, where bending is the greatest.
- The peak value from ANSYS is used as the input for:
- Strength safety factor 
- Fracture toughness safety factor 
- Fatigue safety factor 
- The stress distribution shows a smooth bending profile with no unexpected stress concentrations, indicating that the geometry transitions and fillets are functioning as intended.

The maximum stress value extracted from this plot is compared directly to material allowables (yield strength, fatigue limit, and effective fracture toughness) to validate the final design.

---

## Summary of FEM Results

From the ANSYS simulation of the torque wrench under the full 600 in-lbf load, the following key values were extracted and used for validation and safety factor calculations:

### FEM Output Values

| Quantity&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Value |
|-------------------------------------|----------------|
| Max normal stress&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | **13,754 psi** |
| Strain at gauge location&nbsp;&nbsp; | **1,146 µε** |
| Deflection at load point&nbsp;&nbsp; | **0.32791 in** |


### Interpretation

- The maximum normal stress is well below the 60,000 psi allowable stress for 2014-T651, indicating adequate strength.
- The gauge strain of 1,146 µε provides a strong signal for the strain-gauge bridge and meets the project sensitivity requirement when converted to mV/V.
- The tip deflection of 0.328 inches is consistent with the large bending moment applied and matches expectations for an aluminum handle of this geometry.
- These FEM values were used directly to compute:
- Strength safety factor 
- Fracture toughness safety factor 
- Fatigue safety factor 
- Sensitivity (mV/V output)

The FEM results closely align with the behavior predicted by beam theory, validating that the selected geometry behaves as expected under the design load.

---

## Sensitivity Calculation (mV/V)

Using the strain at the gauge obtained from ANSYS, the torque wrench sensitivity was computed based on a half-bridge strain-gauge configuration.

### Inputs
- Strain at gauge: **1,146 µε**
- Gauge factor (GF): **2.0**
- Bridge configuration: **Half-bridge** → bridge factor = **1/2**

### Calculation
Sensitivity is estimated using:

mV/V output ≈ GF × strain × bridge factor

Substituting the values:

mV/V output ≈ 2.0 × 1,146 µε × (1/2) 
mV/V output ≈ **1.146 mV/V**

### Result
The resulting sensitivity is:

- **1.146 mV/V**

This confirms that the final design meets the required minimum electrical sensitivity for accurate torque measurement.

---

## Strain Gauge Selection

To measure the bending strain in the wrench handle, I selected a standard linear foil strain gauge suitable for aluminum components. The gauge is bonded to the top and bottom surfaces of the handle in a half-bridge configuration to measure tension and compression during bending.

### Gauge Details
- **Type:** Linear foil strain gauge 
- **Resistance:** 120 Ω 
- **Gauge factor (GF):** 2.0 
- **Gauge orientation:** Aligned with the longitudinal axis of the handle (bending direction) 
- **Bridge configuration:** Half-bridge (one gauge in tension, one in compression)

### Placement Justification
- The gauges are placed **1 inch from the drive**, where the bending moment is high and the strain field is smooth and uniaxial.
- ANSYS strain contours confirm that this region experiences a consistent normal strain aligned with the gauge direction.
- The flat surface of the handle provides adequate bonding area for a standard gauge footprint, ensuring full adhesive contact and accurate strain transfer.
- A half-bridge configuration doubles the output signal compared to a single-gauge (quarter-bridge) arrangement and provides temperature compensation.

### Why This Gauge Works for the Design
- The measured FEM strain of **1,146 µε** produces a sensitivity of **1.146 mV/V**, exceeding the project minimum requirement of **1.0 mV/V**.
- The gauge orientation captures bending strain directly, which is the dominant deformation mode in the handle.
- The selected gauge is compatible with aluminum and standard adhesive systems used in strain measurement.