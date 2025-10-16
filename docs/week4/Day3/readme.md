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

---


# CMOS Inverter – Switching Threshold (VM) Analysis

This section explains the **Switching Threshold (VM)** of a CMOS Inverter — a key parameter that defines the inverter’s static robustness and determines the symmetry of its Voltage Transfer Characteristic (VTC).

## 1. Switching Threshold (VM) Definition

- The **Switching Threshold (VM)** is the input voltage at which the output voltage is **exactly equal** to the input voltage.

$$
\mathbf{V_M: \; V_{in} = V_{out}}
$$

- On the **VTC**, this is the point where the curve intersects the line $V_{out} = V_{in}$.
- It typically lies in the **high-gain region**, where the output transitions rapidly.

**Significance:**  
For an ideal symmetric inverter:  

$$
V_M = \frac{V_{DD}}{2}
$$

A $V_M$ close to $V_{DD}/2$ ensures **equal noise margins** for logic ‘1’ and logic ‘0’, resulting in robust operation.

---

## 2. Derivation Based on Current Equality

The fundamental DC condition at the output node is based on **Kirchhoff’s Current Law (KCL):**

$$
\mathbf{I_{dsP} + I_{dsN} = 0} \quad \Rightarrow \quad \mathbf{I_{dsP} = -I_{dsN}}
$$

At the **switching threshold**, $V_{in} = V_{out} = V_M$.  
Both transistors conduct **equal and opposite** currents.


### **Operating Regions at VM**

When $V_{in} = V_{out} = V_M$:

| Transistor | Gate-Source Voltage | Drain-Source Voltage | Region |
|-------------|---------------------|-----------------------|---------|
| NMOS | $V_{GSN} = V_M$ | $V_{DSN} = V_M$ | Saturation |
| PMOS | $V_{GSP} = V_M - V_{DD}$ | $V_{DSP} = V_M - V_{DD}$ | Saturation |

Both are typically in the **saturation region**, which simplifies the derivation.


### **Full Current Equation**

The images show a general current equation assuming both transistors are near the **saturation–linear boundary** using an effective $V_{DSAT}$ term:

$$
\mathbf{ k_p \left[ (V_M - V_{DD} - V_t) V_{dsatp} - \frac{V_{dsatp}^2}{2} \right] + k_n \left[ (V_M - V_t) V_{dsatn} - \frac{V_{dsatn}^2}{2} \right] = 0}
$$


Where:
- $k_n, k_p = \mu C_{ox} (W/L)$ → Transistor gain factors  
- $V_{dsatn}, V_{dsatp}$ → Effective drain-source saturation voltages  
- $V_t$ → Threshold voltages  

This nonlinear equation must be solved for the **unknown $V_M$**.

---

## 3. Dependence on Transistor Sizing (Beta Ratio)

Since solving the full equation is complex, a simplified **ratio-based expression** is often used to show the dependence of $V_M$ on device sizes.

### **Simplified VM Equation**

$$
\mathbf{V_M = R \cdot \frac{V_{DD}}{1 + R}}
$$

Where

$$
\mathbf{R = \frac{\beta_n}{\beta_p}}
$$  

is the **transistor strength ratio (beta ratio)**.

### **Expanded Beta Ratio Expression**

$$
\mathbf{
R = \frac{K_n' (W_n / L_n) V_{dsatn}}
{K_p' (W_p / L_p) V_{dsatp}}
}
$$

- $K_n' = \mu_n C_{ox}$, $K_p' = \mu_p C_{ox}$  
- If $L_n = L_p$, then $R$ depends mainly on the **width ratio ($W_p / W_n$)**.


### **Design for Symmetric VM**

To achieve an ideal switching point at:

$$
V_M = \frac{V_{DD}}{2}
$$

The ratio $R$ must satisfy:

$$
\frac{V_{DD}}{2} = R \cdot \frac{V_{DD}}{1 + R}
\quad \Rightarrow \quad R = 1
$$

However, since **electron mobility ($\mu_n$)** is ~2–3× higher than **hole mobility ($\mu_p$)**,  
the **PMOS must be wider** to balance current drive:

$$
\frac{W_p}{W_n} \approx 2 \text{ to } 3
$$

---

## 4. Example Results (VDD = 2V)

| Sizing | $W_p / W_n$ | $V_M$ | Comment |
|---------|--------------|-------|----------|
| $W_n/L_n = 1.5$, $W_p/L_p = 1.5$ | 1 | $V_M ≈ 0.98V$ | Too low |
| $W_n/L_n = 1.5$, $W_p/L_p = 3.75$ | 2.5 | $V_M ≈ 1.2V$ | Closer to $V_{DD}/2 = 1V$ |

---

### **Summary**

- $V_M$ defines the inverter’s **switching balance** and **noise margins**.  
- It depends strongly on the **PMOS-to-NMOS size ratio**.  
- For **symmetric operation**, design $W_p/W_n ≈ 2$–3.  
- Accurate $V_M$ estimation ensures **robust logic level restoration** in cascaded CMOS circuits.



