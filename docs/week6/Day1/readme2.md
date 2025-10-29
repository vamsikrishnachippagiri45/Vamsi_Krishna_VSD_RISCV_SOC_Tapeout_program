# Introduction: OpenLANE

---

## OpenLANE
* **Definition:** OpenLANE is an **open-source flow** designed for a **True Open Source Tape-out Experiment** (meaning creating a manufacturable chip using only open-source tools and data).
* **Main Goal:** To **produce a clean GDSII** (the final chip layout file) **with no human intervention** ("no-human-in-the-loop").
* **Technology:** It's **tuned for the SkyWater 130nm Open PDK** and is distributed in a **containerized** format, making it functional "out of the box."
* **Functionality:** It can be used to "harden" (create the physical layout for) both **Macros** (large circuit blocks) and full **Chips**.


## striVe SoC Family
* **Concept:** **striVe** is a family of **"open everything SoCs,"** representing the full open-source silicon stack: **Open PDK, Open EDA, and Open RTL**.
* **Architecture:** The core design uses the **RISC-V 32-bit architecture** (e.g., the `strive_soc` core) and includes peripherals like SRAM, GPIO, and Analog/Digital PLLs.
* **Family Variations:** The striVe family demonstrates different features and design styles, all built using the open-source tools and flow:
    * **striVe:** The foundational chip (Sky130 SCL + Synthesized 1 KBytes SRAM).
    * **striVe 2:** Adds a $1 \text{KByte}$ **OpenRAM** block.
    * **striVe 3:** Uses the **OSU SCL** (Standard Cell Library) instead of Sky130's, along with synthesized SRAM.
    * **striVe 5:** Features multiple $8 \times 1 \text{KByte}$ **OpenRAM banks**.
    * **striVe 6:** Includes **DFT (Design For Testability)** features.
* **Key Partners:** The project is supported by major contributors to open-source silicon, including **SkyWater, Google, OpenROAD, and eFabless**.

Referenec : https://github.com/efabless/openlane2/tree/main?tab=readme-ov-file

---

##  OpenLANE ASIC Flow

The OpenLANE flow is an automated, open-source methodology for converting RTL code into a final GDSII file. It relies heavily on open-source tools like Yosys, OpenSTA, and the OpenROAD app.

### I. The Core Flow

The flow is a loop-based process guided by the **SKY130 PDK** and consists of two main sections:

1.  **Logical & Synthesis:**
    * **RTL Synthesis (Yosys + ABC):** Converts the high-level RTL code into a gate-level netlist. This stage includes **Synthesis Exploration**, where various parameters are tested to find the best balance between area ($\mu\text{m}^2$) and speed (Delay in ps).
2.  **Physical Implementation (OpenROAD App):** Also known as **Automated Place and Route (PnR)**.
    * **Floor/Power Planning:** Defines chip dimensions, I/O pin locations, and the power delivery network (power rings/straps).
    * **Placement (Global and Detailed):** Places all standard cells in the core area. Includes **Post Placement Optimization** to improve timing and reduce wire length.
    * **Clock Tree Synthesis (CTS):** Builds the clock distribution network with minimal skew.
    * **Routing (TritonRoute):** Implements the metal interconnects (wires) in global and detailed stages.

### II. Critical Verification and Optimization Steps

| Step/Tool | Purpose | Details |
| :--- | :--- | :--- |
| **Logic Equivalence Check (LEC)** | **Formal Verification** to ensure the design's function has **not changed** after netlist modifications. | Must be performed every time the netlist is altered, specifically after: **CTS** and **Post Placement Optimizations**. Uses the tool **Yosys**. |
| **Design for Test (DFT)** | Inserts structures into the design to allow for testing the manufactured chip for faults. | Involves **Scan Insertion**, **Automatic Test Pattern Generation (ATPG)**, Fault Coverage, and Fault Simulation. Uses the tool **Fault**. |
| **Static Timing Analysis (STA)** | Verifies that all circuits in the chip will operate at the required clock frequency. | Calculates worst-case path delays. Requires **RC Extraction (DEF2SPEF)** to calculate resistance (R) and capacitance (C) of the wires accurately. Uses the tool **OpenSTA (OpenROAD)**. |
| **Design Exploration** | Automates the process of finding the optimal trade-offs for a design. | OpenLANE runs on many designs (e.g., $\sim 70$ designs) and compares the results to find the **best configurations** for parameters like cell count, utilization, and routing strategy. Also used for **Regression Testing (CI)**. |


### III. Antenna Rules Violations (Manufacturing Concern)

* **Problem:** During fabrication, long metal wires (antennas) can accumulate static charge, which damages the tiny transistor gates (the chip inputs) they are connected to.
* **Preventive Approach (OpenLANE):**
    1.  **Add a Fake Antenna Diode** next to every cell input after placement.
    2.  Run the **Antenna Checker (Magic)** on the routed layout.
    3.  If the checker reports a violation, the **Fake Diode Cell is replaced by a real diode** (provided by the SCL) that leaks away the excess charge, protecting the transistor gate.

### IV. Final Sign Off

* **RC Extraction:** Calculates the resistance (R) and capacitance (C) of the actual routed wires.
* **STA (Final):** Runs timing analysis using the actual R/C values to confirm timing closure.
* **Physical Verification (Magic & netgen):** Final checks for design rule adherence (DRC) and layout-to-schematic matching (LVS).
* **GDSII Streaming (magic):** The final, verified layout is saved as a **GDSII** file, which is the industry standard format sent to the foundry for manufacturing.

---


