# Cell design

---

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


