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
| **Threshold Voltage ($V_t$)** | $V_t = V_{t0} + \gamma (\sqrt{-2\Phi_f + V_{sb}} - \sqrt{-2\Phi_f})$ | Depends on substrate bias $V_{SB}$ and body effect coefficient $\gamma$. |
| **Body Effect Coefficient ($\gamma$)** | $$\gamma = \frac{ \sqrt{2 q \epsilon_{si} N_A} }{ C_{ox} }$$ | Depends on substrate doping $N_A$ and oxide capacitance $C_{ox}$. |
| **Fermi Potential ($\Phi_f$)** | $$\Phi_f = -\Phi_T \ln \left( \frac{N_A}{n_i} \right)$$ | Determined by substrate doping $N_A$ and intrinsic carrier concentration $n_i$. |
| **Drain Current ($I_D$)** | Linear region: $$I_D = k_n[(V_{GS} - V_t)V_{DS} - \tfrac{V_{DS}^2}{2}]$$ <br> Saturation region: $$I_D = \tfrac{1}{2} k_n (V_{GS} - V_t)^2 (1 + \lambda V_{DS})$$ | $k_n = \mu_n C_{ox} \tfrac{W}{L}$, $\lambda$ = channel-length modulation parameter. |

These parameters are defined in the **Technology File** (also called *Model* or *Library File*).

#### Technology File Inclusion

SPICE relies on device models that describe how a transistor behaves electrically. These models contain parameters extracted from the semiconductor fabrication process (e.g., 0.25 µm or 1 µm technology).

To include these models, the netlist links an external technology file using directives like .LIB or .INCLUDE.
These statements tell SPICE where to find the process-specific transistor parameters that define threshold voltage, carrier mobility, oxide thickness, and other physical effects.

The technology file (for example, xxxx_025um_model.mod) contains several .MODEL statements.

These parameters link physical fabrication characteristics (like doping and oxide thickness) to the electrical equations that SPICE uses internally to compute drain current $I_D$, threshold voltage $V_t$, and other transistor behaviors.


