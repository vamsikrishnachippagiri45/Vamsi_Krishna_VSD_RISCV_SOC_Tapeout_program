# CMOS Inverter – DC Transfer Characteristics

This document explains the **DC (Direct Current) Transfer Characteristics** and **Load Line Analysis** of a CMOS (Complementary Metal-Oxide-Semiconductor) Inverter. This analysis is fundamental in **digital VLSI design**.

---

## 1. CMOS Inverter Circuit and Voltage/Current Relations

### **CMOS Inverter Circuit**

<img width="514" height="500" alt="image" src="https://github.com/user-attachments/assets/02f743d1-7528-40e5-aaf7-22fa33ff3884" />


- The inverter consists of a **PMOS (top)** and an **NMOS (bottom)** connected in series.  
- **VDD** (supply voltage) → PMOS source (S).  
- **VSS (Ground, 0V)** → NMOS source (S).  
- **Input (Vin)** → both gates (G).  
- **Output (Vout)** → both drains (D).  
- A capacitor **(CL)** represents the **load capacitance** at the output node (for dynamic behavior).



### **Voltage Relations**

| Parameter | NMOS Transistor | PMOS Transistor | Notes |
|------------|----------------|----------------|--------|
| Gate-to-Source Voltage (VGS) | VGSN = Vin - VSS = Vin | VGSP = Vin - VDD | VSS = 0 |
| Drain-to-Source Voltage (VDS) | VDSN = Vout - VSS = Vout | VDSP = Vout - VDD | VDSN positive, VDSP negative |



### **Current Relation (DC Condition)**

At the output node (using KCL):

IdsP + IdsN + IL = 0

For **DC analysis** (steady state, IL = 0):

IdsP = -IdsN  →  |IdsP| = IdsN

- IdsN is defined positive (flowing from Vout → VSS).  
- IdsP is negative (flowing from VDD → Vout).  



### **Equivalent Circuits (Dynamic Context)**

| Input Condition | NMOS | PMOS | Output Behavior |
|-----------------|------|------|-----------------|
| Vin = VDD | ON (Low Rn) | OFF (Open) | CL discharges to 0 |
| Vin = 0 | OFF (Open) | ON (Low Rp) | CL charges to VDD |

---

## 2. MOSFET ID–VDS Characteristics (Load Curves)

### **NMOS Load Curve**

- Plot: **IdsN vs. Vout**
- Parameter: **Vin (or VGSN)** varies from 0 → VDD.
- Behavior:
  - For Vin > Vtn: current increases (Triode → Saturation).
  - For Vin < Vtn: **Cutoff region** (IdsN ≈ 0).



### **PMOS Load Curve Transformation**

- Original PMOS relation: VDSP = Vout - VDD  
- For plotting on same axes:
  - Vout = VDD + VDSP  
  - When Vout = 0 → VDSP = -VDD  
  - When Vout = VDD → VDSP = 0  
- The transformed curve uses:
  - **x-axis:** Vout (0 → VDD)  
  - **y-axis:** |IdsP| = -IdsP  
- Corresponds to IdsN direction (for KCL consistency).

<img width="300" height="185" alt="image" src="https://github.com/user-attachments/assets/adc092f5-4d07-48b8-baaa-6688d9a5bd35" />


### **PMOS Behavior Summary**

| Vin | PMOS Condition | Action |
|------|----------------|--------|
| 0 | Strongly ON | Pulls Vout high |
| VDD | OFF (Cutoff) | No current flow |


## 3. Graphical DC Transfer Characteristic (Load Line Analysis)

### **Load Line Concept**

- Both **NMOS** and **PMOS load curves** (IdsN vs. Vout and |IdsP| vs. Vout) are plotted together.  
- The **intersection points** of corresponding Vin curves give the **DC operating points**:
  IdsN = |IdsP|



### **Voltage Transfer Characteristic (VTC)**

- Plot: **Vout vs. Vin**  
- Shows inverter’s switching behavior:
  - Output transitions from **High (VDD)** → **Low (0V)**  
  - As input increases from 0 → VDD



### **Five Regions of Operation**

<img width="314" height="214" alt="image" src="https://github.com/user-attachments/assets/e8e5137c-35dd-4f4a-98d4-1581ef6c20b2" />



| Region | Vin Range | NMOS Mode | PMOS Mode | Vout | ID Behavior |
|--------|------------|------------|------------|--------|--------------|
| A | Vin ≈ 0 | Cutoff | Linear | VDD (High) | ≈ 0 |
| B | Low Vin | Saturation | Linear | Decreasing | Increasing |
| C | Vin ≈ VM | Saturation | Saturation | Rapid drop | Max pulse |
| D | High Vin | Linear | Saturation | Decreasing | Decreasing |
| E | Vin ≈ VDD | Linear | Cutoff | 0 (Low) | ≈ 0 |



### **Key Points**

- In steady states (**Regions 1 & 5**), one transistor is **OFF**, so **static power ≈ 0**.  
- During transition (**Regions 2–4**), both transistors conduct → **short-circuit current pulse**.  
- The **VTC** has a sharp slope (high gain) around the **switching point (VM)** — essential for noise immunity and logic robustness.



