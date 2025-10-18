# CMOS Inverter Robustness – (iii) Power Supply Scaling


Based on the provided images and SPICE netlist for the CMOS inverter robustness evaluation with power supply scaling, here are the notes for the case where the power supply voltage ($V_{DD}$) is **$0.5\text{V}$**.

## CMOS Inverter Robustness: Power Supply Scaling to $0.5\text{V}$

The SPICE simulation explores the effect of scaling down the power supply voltage ($V_{DD}$) on a CMOS inverter, specifically highlighting the advantages and disadvantages when $V_{DD}$ is reduced to $0.5\text{V}$. The DC characteristics plot shows multiple transfer curves corresponding to $V_{DD}$ values from $2\text{V}$ down to $0.5\text{V}$ (labeled as **dc1 out** through **dc4 out**). The curve corresponding to the lowest $V_{DD}$ is the one of interest.

### 1. Advantages of Using $0.5\text{V}$ Supply

The most significant benefit of power supply scaling is the reduction in **dynamic power consumption** and an unexpected **increase in voltage gain**.

* **Significant Reduction in Energy/Power:**
    * **Energy Formula:** The dynamic energy consumed per switching event is approximated by the formula: $E = \frac{1}{2} C V_{DD}^2$, where $C$ is the load capacitance and $V_{DD}$ is the supply voltage.
    * **Energy Reduction:** Scaling $V_{DD}$ from a baseline (e.g., $2\text{V}$ or a higher voltage not explicitly shown, but implied by the context of scaling) to $0.5\text{V}$ results in a quadratic reduction in dynamic energy.
    * **Power Reduction:** Dynamic power, $P_{dynamic} = E \cdot f \cdot \alpha$ (where $f$ is frequency and $\alpha$ is activity factor), also decreases quadratically with $V_{DD}$.

* **Increase in Voltage Gain:**
    * The voltage gain ($A_v$) is the slope of the DC transfer characteristic in the transition region.
    * **Measured Gain:** The simulation results show that for $V_{DD} = 0.5\text{V}$, the gain is **$5.81$**.
    * **Improvement:** A substantial improvement in gain, showing a $50\%$ improvement from Vdd = 2V.
    * **Reason:** At low $V_{DD}$, the drain-source voltage ($V_{DS}$) is small, pushing the MOSFETs deeper into the **saturation region** (for a brief range) where the transconductance ($g_m$) is high, leading to a larger gain: $A_v \approx -g_{m}(R_{out})$.

---

### 2. Disadvantages and Performance Impact

While energy is significantly reduced, the major drawback of scaling $V_{DD}$ down to $0.5\text{V}$ is the **impact on circuit performance**, primarily in terms of speed.

* **Performance Impact (Increased Delay):**
    * The **rise delay** and **fall delay** of the inverter, which together determine the circuit's operating speed, are negatively affected.
    * **Reason:** The charge/discharge current ($I$) available to charge the load capacitance ($C_{load}$) is reduced with lower $V_{DD}$, as $I \propto (V_{DD} - V_{th})^2$ (where $V_{th}$ is the threshold voltage).
    * Since delay ($\tau$) is proportional to $C_{load} V_{DD} / I$, a reduction in $V_{DD}$ and the corresponding reduction in current $I$ (which is a strong function of $V_{DD}$) leads to a significant **increase in propagation delay**.
      
* **Increased Sensitivity to Noise and Process Variation:**
    * As the difference between $V_{DD}$ and the transistor threshold voltage ($V_{th}$) shrinks, the circuit becomes more susceptible to **noise** (reducing the noise margin) and **process variations** (making the performance less predictable across manufactured chips).

---

## Spice simulation
### Netlist 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.495um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 0.1fF

Vdd vdd 0 2V
Vin in 0 2V

.control 
let powerSupply = 2 
alter Vdd = powerSupply

    let voltageSupplyVariation = 0
    dowhile voltageSupplyVariation < 4
        dc Vin 0 2 0.01
        let powerSupply = powerSupply - 0.5
        alter Vdd = powerSupply
        let voltageSupplyVariation = voltageSupplyVariation + 1 
    end
    
    plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in  xlabel "Input Voltage[V]" ylabel "Output Voltage[V]" title "Inverter DC Characteristics as a function of supply voltage"
    * quit 
.endc 
.end
```

<img width="746" height="684" alt="image" src="https://github.com/user-attachments/assets/044f7960-1374-46a8-b70d-0fb6c67a96d7" />

for vdd = 2v , gain = 2.9
for vdd = 1.5v , gain = 3.6
for vdd = 1v , gain = 5.67
for vdd = 0.5v , gain = 5.81

---

# CMOS Inveter Robustness : (iv) Device Variation

# Source of Variation in fabrication : Etching process

The etching process in semiconductor fabrication introduces **physical variations** in transistor dimensions, leading to deviations in electrical behavior. This affects performance metrics such as **delay**, **power**, and **noise margins**.

## 1. Impact of Non-Ideal Etching

- **Ideal Mask:** Defines perfect, rectangular gate features with specified channel **Length (L)** and **Width (W)**.  
- **Actual Mask (Fabricated Device):** Exhibits **ragged and non-uniform edges**, primarily due to:
  - Non-uniform chemical reactions
  - Over-etching
  - Lithography alignment errors

### **Dimensional Deviation**
- Fabricated values of **L** and **W** deviate from their designed mask values.
- This causes the transistor geometry to become **imprecise**, leading to mismatch and performance variation.



## 2. Effect on Transistor Current

The **drain current (Id)** in a MOSFET is strongly dependent on its geometry, specifically the **aspect ratio**:

$$ \[
I_d \propto \left(\frac{W}{L}\right)
\] $$

### **Key Points**
- Since **etching imperfections** alter both **W** and **L**, the **(W/L)** ratio varies.  
- This directly affects the **drive current (Id)**, causing:
  - **Speed variation:** changes in propagation delay  
  - **Power variation:** due to different drive strengths  
  - **Mismatch:** between identically designed transistors


## 3. Positional Variation in an Inverter Chain

### **Middle Gates**
- Transistors located in the **middle of diffusion lines** have **symmetric surroundings**.  
- Result: **Uniform etching** → consistent dimensions and electrical behavior.

### **End Gates**
- Gates at the **ends of diffusion regions** experience **asymmetric etching environments**.
- Causes:
  - Easier access for etchant
  - Over-etching at boundaries  
- Result: **Larger dimensional variation** in channel length (**L**) and width (**W**) compared to middle transistors.

---

#  Sources of Variation – Oxide Thickness ($t_{ox}$)

This section explains how variations in **gate oxide thickness ($t_{ox}$)** during the CMOS fabrication process affect the electrical performance of MOSFETs and, consequently, digital circuits such as inverter chains.

##  1. Variation in Gate Oxide Thickness

The gate oxide forms a critical insulating layer between the gate terminal and the silicon channel.  
However, **fabrication imperfections** cause non-uniformity in this layer, leading to **device-to-device variability**.

###  Ideal vs. Actual Oxide Layer

| Property | Ideal Case | Actual Fabricated Case |
|-----------|-------------|------------------------|
| Oxide Thickness ($t_{ox}$) | Perfectly uniform | Non-uniform and rough |
| Surface Quality | Smooth and consistent | Rough due to etch/deposition variations |
| Result | Consistent transistor behavior | Variability in $C_{ox}$ and $I_d$ |

- **Ideal Layer:**  
  Uniform gate oxide with constant thickness $t_{ox}$ across the wafer.
- **Actual Layer:**  
  Due to non-uniform thermal oxidation or CVD deposition, the oxide layer thickness varies spatially.
- **Consequence:**  
  The effective gate oxide thickness ($t_{ox}$) differs from the intended design value, causing electrical parameter variation across transistors.


##  2. Impact on Transistor Electrical Characteristics

The gate oxide thickness directly determines the **gate oxide capacitance ($C_{ox}$)**, which controls how effectively the gate voltage modulates the channel.

###  Gate Oxide Capacitance

$$
C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}
$$

where:
- $\varepsilon_{ox}$ = Permittivity of silicon dioxide ($\text{SiO}_2$)  
- $t_{ox}$ = Physical oxide thickness  

A **thicker oxide** → lower $C_{ox}$ → weaker gate control  
A **thinner oxide** → higher $C_{ox}$ → stronger gate control  


### Dependence of Drain Current ($I_d$) on $t_{ox}$

In the **linear region**, the drain current of a MOSFET is given by:

$$
I_d = \mu \frac{\varepsilon_{ox}}{t_{ox}} \left( \frac{W}{L} \right)
\left[ (V_{gs} - V_t)V_{ds} - \frac{V_{ds}^2}{2} \right]
$$

- $\mu$ → Carrier mobility  
- $\frac{W}{L}$ → Aspect ratio of the transistor  
- $V_t$ → Threshold voltage  

### Key Observations:
- $I_d \propto \frac{1}{t_{ox}}$ → As $t_{ox}$ increases, $I_d$ decreases.  
- Variation in $t_{ox}$ leads to **unequal $I_d$** across transistors → impacts delay and power.  
- Changes in $t_{ox}$ also alter $V_t$, compounding variation effects.


##  3. Application: Inverter Chain Layout

The lower schematic shows an **inverter chain** layout (CMOS inverters connected in series).  
Each inverter consists of a **PMOS** and **NMOS** transistor, both affected by oxide thickness variation.

### Effects in the Inverter Chain:
- Each transistor’s drive strength varies depending on its local $t_{ox}$.  
- This causes **propagation delay mismatch** and **timing skew** between stages.  
- Power and speed vary non-uniformly across the chip.

---

# Spice simulations for device variations

This section summarizes how varying PMOS ($W_p$) and NMOS ($W_n$) widths affects inverter behavior, based on SPICE DC transfer simulations.

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.195um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 10fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control 
    let nmoswidth = 0.195u
    alter M2 w=nmoswidth
    
    let pmoswidth = 0.975u
    alter M1 w=pmoswidth

    let widthvariation = 0
    dowhile widthvariation < 5
        dc Vin 0 1.8 0.01
        let nmoswidth = nmoswidth + 0.195u
        let pmoswidth = pmoswidth - 0.195u
        alter @M2[w] = nmoswidth
        alter @M1[w] = pmoswidth
        let widthvariation = widthvariation + 1 
    end
    
    plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in xlabel "Input Voltage[V]" ylabel "Output Voltage[V]" title "Inverter DC Characteristics as function of nmos & pmos widths"
.endc 
.end
```

<img width="1163" height="874" alt="image" src="https://github.com/user-attachments/assets/1297adb3-00a1-40ab-bb7f-df6fe51ab6aa" />

### 1. Simulation Setup

The simulation studies inverter characteristics under different $\frac{W_p}{W_n}$ ratios.

- **Technology:** 130 nm  
- **$V_{DD}$:** 1.8 V  
- **$L_p = L_n$:** 130 nm (fixed)  
- **Width Sweep:**  
  - $W_n$ increased in 0.195 μm steps  
  - $W_p$ decreased in 0.195 μm steps  

| Run | $W_n$ (μm) | $W_p$ (μm) | $\frac{W_p}{W_n}$ | Drive Strength | Color |
|:--:|:--:|:--:|:--:|:--:|:--:|
| dc1.out | 0.195 | 0.975 | 5.0 | Strong PMOS | 🟩 Green |
| dc2.out | 0.390 | 0.780 | 2.0 | Balanced | 🟨 Yellow |
| dc3.out | 0.585 | 0.585 | 1.0 | Weaker PMOS | 🟥 Red |
| dc4.out | 0.780 | 0.390 | 0.5 | Strong NMOS | 🟦 Blue |
| dc5.out | 0.975 | 0.195 | 0.2 | Very Strong NMOS | 🟪 Magenta |


###  2. Key Observations

**1. Switching Threshold ($V_m$) Shift**
- $V_m$ ≈ input voltage where $V_{out} = V_{in}$  
- Depends on PMOS/NMOS strength balance  
  - Large $\frac{W_p}{W_n}$ → $V_m$ shifts **right** (higher $V_{in}$)  
  - Small $\frac{W_p}{W_n}$ → $V_m$ shifts **left** (lower $V_{in}$)  

**2. Noise Margins ($NM_H$, $NM_L$)**
- Strong PMOS → ↑ $NM_H$, ↓ $NM_L$  
- Strong NMOS → ↓ $NM_H$, ↑ $NM_L$  
- The transfer curve moves horizontally, altering the balance of high/low noise margins.


###  3. Design Implications

- **Balanced Design:** Ideal $\frac{W_p}{W_n} \approx 2.0$ gives $V_m \approx V_{DD}/2$ (dc2.out).  
- **Process Sensitivity:** Variation in $W_p$, $W_n$ shifts $V_m$ → unbalanced noise margins.  
- **Impact:** Excessive mismatch reduces inverter robustness and can cause unreliable logic levels.


###  Supporting Relation

At switching threshold ($V_M$):

$$
k_p \Big[(V_M - V_{DD} - V_t)V_{dsatp} - \tfrac{V_{dsatp}^2}{2}\Big] 
+ k_n \Big[(V_M - V_t)V_{dsatn} - \tfrac{V_{dsatn}^2}{2}\Big] = 0
$$

