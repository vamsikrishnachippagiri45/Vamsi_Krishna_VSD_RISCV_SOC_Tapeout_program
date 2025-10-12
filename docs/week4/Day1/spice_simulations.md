# Spice simulation 


## Introduction to SPICE Simulation

SPICE (Simulation Program with Integrated Circuit Emphasis) is an industry-standard circuit simulator developed at UC Berkeley for analog and mixed-signal analysis. It solves circuit equations (using nodal analysis and Newton-Raphson methods) from a netlist to find voltages and currents at each node.

Types of Analyses:
DC Analysis: Finds steady-state operating point , 
AC Analysis: Analyzes frequency response (filters, amplifiers) , 
Transient Analysis: Studies time-domain behavior (waveforms, rise/fall times).

Why SPICE Simulations Are Needed in IC Design : 

1) To Understand Cell Delay:
Delay comes from internal transistor switching and output load capacitance (fanout + interconnects).
SPICE uses accurate transistor models to calculate true propagation delay under specific conditions (input slew & output load).

2) To Ensure Delay Models Are Accurate:
STA tools use pre-characterized delay tables (.lib) generated using SPICE simulations.
SPICE is the “gold standard” for building these models.

3) To Verify STA Results (Signoff):
SPICE verifies critical timing paths detected by STA.
Final signoff simulations (FastSPICE/full SPICE) ensure accurate timing before tapeout.

---


## NMOS SPICE Simulation Setup


### 1. SPICE Simulation Overview

SPICE combines the circuit description with device models to solve nonlinear differential equations that describe the circuit behavior.

| **Component** | **Description** | **Role in Simulation** |
|:--------------|:----------------|:------------------------|
| **SPICE Netlist** | Text file defining the circuit’s connectivity (nodes and components such as M1, R1, Vdd, Vin). | Defines what the simulator solves. |
| **SPICE Model Parameters** | Physical and electrical parameters ($V_{T0}$, $\gamma$, $\Phi_f$, $\lambda$, $\mu_n$, $C_{ox}$) from process technology. | Defines how each transistor behaves. |
| **SPICE Software** | The solver engine combining netlist, model, and analysis commands. | Performs numerical simulation. |
| **Output** | I–V characteristics (e.g., $I_D$ vs $V_{DS}$) or transient waveforms. | Visualizes results and verifies behavior. |


### 2. SPICE Model Parameters & Key Equations

The accuracy of the simulation depends on parameters linking process technology to device physics.

| **Parameter** | **Governing Equation** | **Source of Value** |
|:---------------|:------------------------|:--------------------|
| **Threshold Voltage ($V_t$)** | $$V_t = V_{t0} + \gamma \left( \sqrt{ -2\Phi_f + V_{SB} } - \sqrt{| -2\Phi_f |} \right)$$ | Depends on substrate bias $V_{SB}$ and body effect coefficient $\gamma$. |
| **Body Effect Coefficient ($\gamma$)** | $$\gamma = \frac{ \sqrt{2 q \epsilon_{si} N_A} }{ C_{ox} }$$ | Depends on substrate doping $N_A$ and oxide capacitance $C_{ox}$. |
| **Fermi Potential ($\Phi_f$)** | $$\Phi_f = -\Phi_T \ln \left( \frac{N_A}{n_i} \right)$$ | Determined by substrate doping $N_A$ and intrinsic carrier concentration $n_i$. |
| **Drain Current ($I_D$)** | Linear region: $$I_D = k_n[(V_{GS} - V_t)V_{DS} - \tfrac{V_{DS}^2}{2}]$$ <br> Saturation region: $$I_D = \tfrac{1}{2} k_n (V_{GS} - V_t)^2 (1 + \lambda V_{DS})$$ | $k_n = \mu_n C_{ox} \tfrac{W}{L}$, $\lambda$ = channel-length modulation parameter. |

These parameters are defined in the **Technology File** (also called *Model* or *Library File*).



### 3.  SPICE Netlist and Technology File

#### A. Netlist Description (Circuit Connectivity)

A **netlist** is a textual representation of the circuit schematic.

| **Netlist Line** | **Component** | **Description** |
|:-----------------|:--------------|:----------------|
| `M1 vdd n1 0 0 nmos W=1.8u L=1.2u` | MOSFET (M1) | Connects Drain (vdd), Gate (n1), Source (0/Vss), and Bulk (0/Vss). Uses model **nmos** with given W/L. |
| `R1 in n1 55` | Resistor (R1) | Connects node **in** and **n1** with 55 Ω. |
| `Vdd vdd 0 2.5` | DC Source (Vdd) | Supplies 2.5 V between node **vdd** and **ground**. |
| `Vin in 0 2.5` | DC Source (Vin) | Applies 2.5 V between node **in** and **ground**. |



#### B. Technology File Inclusion

Technology parameters are linked using `.LIB` or `.INCLUDE` commands:

```spice
.LIB "xxxx_025um_model.mod" CMOS_MODELS
; or
.INCLUDE "xxxx_1um_model.mod"

