# Introduction: OpenLANE and striVe Chipsets

---

## OpenLANE: The Open-Source ASIC Flow
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

---
