# Cell design

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

