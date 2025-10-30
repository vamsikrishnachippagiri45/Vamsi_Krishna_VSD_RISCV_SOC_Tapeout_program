# Standard Cell design in ASIC

---

## Introduction 

## Library Characterization and Modeling

### 1. The Common Element: Gates or Cells

A central principle across all stages of the digital ASIC design flow (from Logic Synthesis to Floorplanning, Placement, and Routing) is the use of **Standard Cells** (or **Gates**).

* **Standard Cells** are the fundamental physical building blocks of the digital chip.

Every single logical element in the final circuit is a physical instance of a cell from the **Standard Cell Library (SCL)**, which is part of the **PDK**.


### 2. The Need for Characterization and Modeling

Because the entire design is built from these cells, the EDA tools must have precise information about how each cell behaves under various conditions. **Library Characterization and Modeling** is the process of generating this information.

* **Goal:** To accurately **model** the timing, power, and noise characteristics of every standard cell in the library. This information is what the EDA tools (like the placement and routing tools) use to ensure the final chip meets its performance and power goals.
* **Process:** The standard cells are simulated extensively across many different operating conditions (voltage, temperature, output load, input slew) to capture their performance data.


### 3. Characterization Concepts (NLDM and CCS)

The precise timing and electrical information is saved in specialized file formats:

* **NLDM (Non-Linear Delay Model):** An older but still widely used model for timing characterization. It stores cell delay and output transition time as two-dimensional tables indexed by **input transition time (slew)** and **output loading capacitance**.
* **CCS (Composite Current Source) Timing, Power, and Noise Characterization:** A more advanced and accurate modeling standard.
    * It models the cell's output driving ability and input loading using current sources and capacitance, respectively.
    * This provides better accuracy for analyzing signal integrity (noise), power consumption, and timing compared to NLDM, which is critical for advanced process nodes.

By characterizing and modeling these cells accurately, EDA tools can predict the behavior of the final chip with high confidence, ensuring the design works correctly the first time it's manufactured.

---


## The Standard Cell Design Flow

The **Cell Design Flow** is a critical, highly-detailed process that happens *before* the main RTL-to-GDSII flow begins. Its output is the **Library** of standard cells used by all subsequent automated tools.

### I. Inputs to the Cell Design Flow

The design of a Standard Cell is constrained by several specific technical requirements and files provided by the foundry.

1.  **Process Design Kits (PDKs):**
    * **DRC (Design Rule Check) & LVS (Layout Versus Schematic) Rules:** These are the strict geometric and electrical rules provided in the PDK's **Tech File**. They dictate minimum feature sizes, spacing, width, and overlap requirements for every layer (e.g., metal, poly, diffusion).
        * *Example:* Rules based on **Lambda ($\lambda$)**, where $\lambda$ is often half the minimum feature size ($L=\lambda/2$).
    * **SPICE Models:** Highly detailed text files (e.g., `nmos.txt`, `pmos.txt`) containing the complex electrical parameters for the transistors (NMOS and PMOS). These models define the mathematical behavior of the transistors, using equations for parameters like **Threshold Voltage ($\text{Vth}$)** and current ($\text{Id}$) in the **Linear** and **Saturation** regions.

2.  **User-Defined Library Specifications:**
    * **Cell Height:** The fixed vertical dimension of all standard cells. This ensures that all cells align perfectly in rows across the chip core.
    * **Supply Voltage ($\text{Vdd}/\text{Vss}$):** The operating voltage for which the cell must be optimized.
    * **Metal Layers & Pin Location:** Specification of which metal layers must be used for power rails and where the input/output pins should be positioned (e.g., M1 for local routing, fixed Vdd/Vss on M2).
    * **Drawn Gate Length:** The specific transistor channel length to be used, which directly impacts speed and power.

These slides complete the overview of the **Cell Design Flow**, detailing the **Design Steps** and the resulting **Outputs** that are fed into the main ASIC flow.

### II. Design Steps

The process of creating a standard cell involves three main phases, moving from theoretical function to a physical, verified layout:

* **Circuit Design:**
    * This is the initial phase where the desired logic function (e.g., $F_{n} = (\overline{B+D}) + \overline{(A+C)} + \overline{E\cdot F}$) is converted into a **transistor-level schematic** (CMOS logic gates).
    * Techniques like **Euler's path** and **series/parallel network analysis** are used to arrange PMOS and NMOS transistors efficiently.
    * The required **Width/Length ($\frac{W}{L}$) ratio** for the PMOS versus NMOS transistors is calculated (e.g., to ensure the switching threshold $V_m$ is set at an optimal point, often $\frac{V_{DD}}{2}$).
* **Layout Design:**
    * This is the "Art of Layout," where the transistor schematic is translated into the physical geometric shapes (polygons) that define the chip layers (diffusion, polysilicon, metal).
    * **Stick diagrams** and adherence to **DRC** (Design Rule Check) constraints from the PDK are critical to create a compact, manufacturable layout.
    * The goal is to physically arrange the transistors and interconnects to minimize area and parasitic effects while ensuring the cell adheres to the fixed **Cell Height**.
* **Characterization:**
    * Once the physical layout is complete and verified, the cell's electrical behavior (timing, power, noise) must be rigorously modeled using simulation tools. This data forms the essential output library for the automated EDA tools.


### III. Outputs of the Cell Design Flow

The Cell Design Flow produces several crucial files and data formats required by the main RTL-to-GDSII flow tools:

* **GDSII (Graphic Data System II):**
    * This is the **physical layout file** containing the geometric mask data for the standard cell. This file is directly used during the **Placement** step.
* **Extracted SPICE Netlist (.cir):**
    * Generated from the final GDSII layout, this netlist includes all the **parasitic resistance and capacitance** of the wires and transistors. It is used for highly accurate electrical simulation.
* **CDL (Circuit Description Language):**
    * A format often used to represent the schematic and netlist of the cell, primarily for **LVS (Layout Versus Schematic)** checks.
* **LEF (Library Exchange Format):**
    * An **abstract physical view** of the cell. It includes the cell's fixed dimensions (**Cell Height**), pin locations, and blockage areas, but *not* the full GDSII geometry. This is the main input used by PnR tools during **Floorplanning and Placement**.
* **Timing, Noise, and Power .libs (Libraries):**
    * These are the characterized data files (e.g., **NLDM** or **CCS**) containing the cell's simulated performance metrics (delay, power consumption, noise sensitivity) under various conditions. These are essential for the **Static Timing Analysis (STA)** and power analysis steps in the main flow.
 
---


## Standard Cell Characterization Flow

**Characterization** is the rigorous process of simulating a cell's electrical behavior across various conditions to create accurate models that the main ASIC design tools (like STA, power analysis, and noise analysis) can use.

### 1. Inputs and Prerequisites

The simulation relies on specific files and commands to accurately model the cell's performance:

| No. | Input Component | Description |
| :---: | :--- | :--- |
| **1** | **Transistor Models (SPICE)** | Complex text files defining the electrical behavior of NMOS and PMOS transistors (e.g., $180\text{nm}$ library). This is the foundation of the simulation. |
| **2** | **Control Statements** | Commands that tell the simulator (e.g., SPICE) what type of analysis to run (e.g., `run`, `.control`, `.endc`). |
| **3** | **Cell-Under-Test (CUT) Circuit** | The **subcircuit** being characterized (e.g., `my_inv` for an inverter), often connected in a ring or back-to-back configuration. |
| **4** | **Subcircuit Definition** | The detailed SPICE netlist for the specific cell, defining its transistors and connectivity within the test harness. |
| **5** | **Voltage Source** | Defines the supply voltage ($\text{Vdd}$) for the cell (e.g., $1.8\text{V}$ DC source). |
| **6** | **Input Stimulus (Pulse)** | A voltage source that provides the dynamic input signal (edge/pulse) to trigger the cell's switching operation. |
| **7** | **Output Load ($\mathbf{C_L}$)** | A load capacitor (e.g., $10\text{fF}$) attached to the output pin ($\text{out1}$) to model the capacitive load the cell will drive in the actual circuit. |
| **8** | **Analysis Parameters** | The specific simulation parameters, such as the duration (`tran 10n 12.4e-9`) and other conditions. |

### 2. The Characterization Process

The process involves the engine (**GUNA**) systematically simulating the cell while varying two key parameters:

* **Input Transition Time (Input Slew):** How quickly the input signal rises or falls.
* **Output Load ($\mathbf{C_L}$):** The capacitance the cell drives.

The simulation runs hundreds or thousands of times across a grid of these parameters, calculating performance metrics for each combination.

### 3. Outputs and Models

The final result of characterization is a set of comprehensive models used by the EDA tools:

* **Timing (.libs):** Models the propagation **delay** and output **slew** of the cell as a function of input slew and output load (e.g., using **NLDM** or **CCS** tables).
* **Power (.libs):** Models the static (leakage) and dynamic (switching) **power** consumed by the cell.
* **Noise (.libs):** Models the cell's susceptibility to noise and its ability to propagate noise.
* **Function:** Formal description of the logical behavior of the cell.

These output files are collectively known as the **Timing, Noise, and Power .libs** and form the basis for all optimization and sign-off checks in the main ASIC flow.

---


## Timing Characterization

Timing characterization is the process of precisely measuring a standard cell's speed to create models for Static Timing Analysis (STA). This involves defining specific voltage **thresholds** to measure two key metrics: **Propagation Delay** and **Transition Time (Slew)**.

### 1. Timing Threshold Definitions

All timing measurements are based on specific voltage levels, usually expressed as a percentage of the supply voltage ($\text{Vdd}$):

| Threshold Parameter | Typical Value | Purpose |
| :--- | :--- | :--- |
| **in\_rise\_thr / in\_fall\_thr** | $\mathbf{50\%}$ of $\text{Vdd}$ | Used to measure the time at the **input** pin for delay calculation. |
| **out\_rise\_thr / out\_fall\_thr** | $\mathbf{50\%}$ of $\text{Vdd}$ | Used to measure the time at the **output** pin for delay calculation. |
| **slew\_low\_rise\_thr / slew\_low\_fall\_thr** | $\mathbf{20\%}$ of $\text{Vdd}$ | The low voltage point used to define the start of a signal edge for **slew** calculation. |
| **slew\_high\_rise\_thr / slew\_high\_fall\_thr** | $\mathbf{80\%}$ of $\text{Vdd}$ | The high voltage point used to define the end of a signal edge for **slew** calculation. |

### 2. Propagation Delay

**Propagation Delay ($t_p$)** is the time it takes for a signal to travel from the input of the cell to the output.

* **Formula:**
  
$$\text{Delay} = \text{time}(\text{out\_thr}) - \text{time}(\text{in\_thr})$$

* **Measurement:**
    1.  Find the time the **input** signal crosses its $50\%$ threshold ($\text{time}(\text{in\_thr})$).
    2.  Find the time the **output** signal crosses its $50\%$ threshold ($\text{time}(\text{out\_thr})$).
    3.  The delay is the difference between the two times.
* **Edge Types:** Delay must be measured for both transitions:
    * **Rise Delay ($\text{t}_{\text{dr}}$):** Input switches low-to-high (or high-to-low if inverter), output rises (e.g., input fall, output rise).
    * **Fall Delay ($\text{t}_{\text{df}}$):** Input switches low-to-high, output falls (e.g., input rise, output fall).
* **Example (Inverter):** For an input falling edge and the output rising edge, the measured delay is

$$\text{time}(\text{out\_rise\_thr}) - \text{time}(\text{in\_fall\_thr})$$.

### 3. Transition Time (Slew)

**Transition Time (Slew)**, often called the **rise time ($t_r$)** or **fall time ($t_f$)**, measures how quickly the signal changes from one logic level to the other.

* **Definition:** Slew is measured between the **$20\%$ and $80\%$** voltage thresholds.
* **Formula (Rise Time):**
  
$$\text{Rise Slew} = \text{time}(\text{slew\_high\_rise\_thr}) - \text{time}(\text{slew\_low\_rise\_thr})$$

* **Importance:**
    * **Input Slew:** Used as a primary input parameter for the timing model (e.g., in NLDM tables). A slow input slew generally leads to a slower propagation delay through the cell.
    * **Output Slew:** The output slew of one cell becomes the input slew for the next cell, propagating the timing characteristic down the path.

The simulation tool systematically performs these delay and slew measurements over a grid of different **Input Slew** values and various **Output Loads ($C_L$)** to generate the comprehensive timing library files.

---
