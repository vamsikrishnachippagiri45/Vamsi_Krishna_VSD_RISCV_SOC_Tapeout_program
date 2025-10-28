# Introduction : How to talk with computers 

---

## Introduction to IC Package

An **Integrated Circuit (IC) package** is the protective casing that encloses the silicon chip (die), providing **mechanical protection**, **electrical connections** to the PCB, and **heat dissipation**.

IC packages are mainly classified into two types based on how they connect to the PCB:

1. **Through-Hole Technology (THT):**
   Components have long pins inserted into holes on the PCB and soldered on the other side. They are easy to handle, durable, and commonly used in prototyping.
   *Example:* **DIP (Dual In-line Package)**

2. **Surface-Mount Technology (SMT):**
   Components are soldered directly onto the PCB surface, allowing compact, high-density designs for modern devices.

   **Common SMT Package Types:**

   * **SOIC (Small Outline IC):** Two rows of gull-wing leads; compact and easy to inspect.
   * **QFP (Quad Flat Package):** Leads on all four sides; supports many connections.
   * **QFN (Quad Flat No-Leads):** No external leads, uses flat pads underneath; very small and suitable for high-frequency applications.
   * **BGA (Ball Grid Array):** Uses solder balls on the underside for hundreds of connections; ideal for processors due to excellent electrical and thermal performance.

In short, IC packaging bridges the tiny internal circuitry of a chip with the larger external world, ensuring protection, connectivity, and efficient heat management.

## 1. The QFN-48 Package
* **Package Type:** **QFN-48** (Quad Flat No-leads)
    * This is a type of surface-mount electronic component package that houses an integrated circuit (IC).
    * **"No-leads"** means the package has connections (pads) on the bottom, not protruding leads like a traditional DIP (Dual In-line Package).
    * **"48"** indicates the total number of connection points (pads) on the exterior of the package.


## 2. Chip and Pinout
* **Chip:** The central component, an **Integrated Circuit (IC)** or **System-on-a-Chip (SoC)**, which performs the main function.
* **Pinout Diagram:** The package has 48 connection points, labeled with their function around the perimeter:
    * **GPIOs (General Purpose Input/Output):** Pins like `gpio0` to `gpio15` are flexible for digital I/O.
    * **Power and Ground:** Pins like `vdd3v3` (3.3V supply), `vdd1v8` (1.8V supply), and `vss` (Ground).
    * **Analog I/O:** Pins like `adc0_in`, `adc1_in`, `analog_out`.
    * **Special Function Pins:**
        * `XO` (Crystal Oscillator)
        * `vref_l`, `vref_h` (Voltage References)
        * `comp_inn`, `comp_inp` (Comparator inputs)
        * `irq` (Interrupt Request)
        * `ser_tx`, `ser_rx` (Serial Transmit/Receive)
        * **Flash I/O/Control:** Pins for external flash memory like `flash_csb`, `flash_clk`, and `flash_io0` to `flash_io3`.
        * **SPI/I2C Control:** Pins like `scl`, `sda`, `sck`, `csb`, `di`, `do` (for protocols like I2C/SPI).
        * `xclk` (External Clock)

## 3. Internal Components: Die, Pads, and Core
* **Pads:** The outermost metal areas where the wires from the IC connect to the package's external pins. They act as the interface between the internal chip wiring and the external world.
* **Die:** The innermost, active area of silicon containing the actual transistors and circuits. It is bounded by the pads.
* **Core:** The main functional block within the die, typically containing the **Processor/SoC** and its primary memory and peripherals. The core is surrounded by the pads.
    * **Connection Path:** Signal wires run from the **Core** to the **Die** edge, where they connect to the **Pads**, which are then wired out to the physical pins of the QFN package.

## 4. Processor/SoC Architecture and IPs
* **Processor/SoC:** The central unit of the system, responsible for execution and control.
* **Macros/Intellectual Properties (IPs):** Pre-designed, reusable functional blocks integrated into the SoC. These are categorized into:
    * **Foundry IPs:** Standard blocks provided by the manufacturing foundry (e.g., `adc`, `dac`, `SRAM`).
    * **Third-party/Internal IPs (Macros):** Larger, complex blocks designed for specific functionality (e.g., **RISC-V SoC**, **GPIO bank**, **SPI** module).
* **System Peripherals and Memory:** The SoC is typically surrounded by:
    * **SDRAM** (Synchronous Dynamic Random-Access Memory)
    * **QSP-Flash** (Quad SPI Flash)
    * **EEPROM** (Electrically Erasable Programmable Read-Only Memory)
    * **JTAG/UART/FTDI** blocks for programming and debugging.
* **Connectivity:** The various IPs and peripherals communicate through the core's buses, with their I/O typically routed to specific **GPIO** pins:
    * Examples: Direct I2C/QSPI/PWM signals are routed through specific GPIO groups (`gpio14-15`, `gpio6-7`, `gpio0-3`, etc.).
    * **ADC (Analog-to-Digital Converter)** functionality is also mixed with a set of GPIO pins.

---

## Instruction Set Architecture (ISA)

### **1. Introduction**

The **Instruction Set Architecture (ISA)** is the **interface between software and hardware**. It acts as a bridge that defines how software communicates with the processor, hiding the physical complexity of hardware design from programmers.

* **Definition:**
  ISA specifies the **instructions**, **data types**, **registers**, **addressing modes**, **memory organization**, and **I/O mechanisms** that a processor supports.
* **Purpose:**
  It allows software to be written independently of the processor’s physical implementation.


### **2. Role of ISA**

The ISA is often referred to as the **"architecture of the computer"** because it defines what the processor can do, not how it does it.

* Acts as an **abstraction layer** between software and hardware.
* Defines the **machine-level instructions** that compilers and assemblers use.
* Ensures **compatibility** between software and hardware of the same ISA family.


### **3. Software-to-Hardware Flow**

The process of converting high-level software into hardware-executable instructions occurs in several stages:

| **Stage**                   | **Component**                                                               | **Function**                                             | **Output**                        |
| --------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------- | --------------------------------- |
| 1. **Application Software** | Programs written in languages like C/C++, Java, Python.                     | Describes the logic or functionality to be performed.    | Source Code (`.c`, `.cpp`)        |
| 2. **System Software (OS)** | Manages hardware resources and provides system calls for program execution. | Handles I/O, memory, and process control.                |                               |
| 3. **Compiler**             | Translates high-level code into assembly language based on the target ISA.  | Converts human-readable logic into ISA-level operations. | Assembly Code (`add x10, x9, 40`) |
| 4. **Assembler**            | Converts assembly instructions into binary machine code.                    | Maps mnemonics to binary opcodes.                        | Machine Code (0s and 1s)          |


### **4. Hardware Implementation**

Once the binary code is generated, it is executed by the hardware according to the rules of the ISA.

* **Instruction Decoding:**
  The processor decodes binary instructions (e.g., RISC-V format) and determines what operation to perform.
* **RTL Description:**
  The hardware behavior is described in **Register Transfer Level (RTL)** code (e.g., in Verilog).

  * Example: The `picorv32` core uses `always` blocks and conditional statements to define how data flows between registers.
* **Synthesis:**
  RTL is converted into a **netlist** that specifies the gates and interconnections implementing each instruction.
* **Physical Design (Layout):**
  The netlist is transformed into the **physical layout**, representing actual transistor and wire placements on silicon.

  * This layout forms the **chip** that executes binary instructions exactly as defined by the ISA.



### **5. Key points**

* The **ISA** defines *what* the processor can do, while the **microarchitecture** defines *how* it does it.
* It provides a consistent programming model despite changes in hardware design.
* The entire process — from source code to layout — ensures that **software operations** are ultimately realized as **hardware actions** on silicon.

**Example ISA:** RISC-V, ARM, x86, MIPS.

---

# Open Source Digital ASIC Design


## Introduction

**ASIC** stands for **Application-Specific Integrated Circuit** (a custom chip). Open Source ASIC design refers to using publicly available, non-proprietary tools and data to create these custom chips.

The three main components of Open Source ASIC design are:

1.  **EDA Tools (Electronic Design Automation):**
    * These are the software tools used to draw, simulate, and verify the chip design.
    * *Examples:* qFlow, OpenROAD, OpenLANE (open-source design flow tools).
2.  **RTL Designs (Register Transfer Level):**
    * This is the functional description of the chip's logic (written in languages like Verilog).
    * *Sources:* librecores.org, opencores.org, GitHub (where the chip logic is openly shared).
3.  **PDK Data (Process Design Kit Data):**
    * This is the critical data that models the actual manufacturing process.
    * **Key Example:** The **Google/SkyWater FOSS 130nm Production PDK** is a famous open-source PDK, enabling anyone to design chips for that specific $130\text{nm}$ fabrication process.

### What is a PDK? (Process Design Kit)

* **PDK** stands for **Process Design Kit**.
* It is a **collection of files** that acts as the "rulebook" for designing an Integrated Circuit (IC) for a specific factory (**fabrication process**).
* **Analogy:** If designing an IC is like building a complex Lego castle, the PDK is the official instruction manual, specifying exactly which bricks (transistors) are available and how close they can be placed without falling apart.
* **What's inside the PDK?**
    * **Process Design Rules:** The strict geometric and electrical rules the design must follow.
        * **DRC (Design Rule Check):** Rules for physical dimensions (e.g., minimum wire width).
        * **LVS (Layout Versus Schematic):** Rules to ensure the physical layout matches the electrical circuit plan.
    * **Device Models:** Files describing how transistors and other components behave electrically.
    * **Digital Standard Cell Libraries:** Pre-designed, ready-to-use logical elements (like AND gates, flip-flops).
    * **I/O Libraries:** Pre-designed circuits for connecting the chip's internal logic to the outside world (pins).

This flow chart details the steps in the **Simplified RTL-to-GDSII Flow**, which is the core process of converting a high-level digital chip description into the physical layout ready for manufacturing.

***

## Simplified RTL-to-GDSII Flow 

This process converts **RTL (Register Transfer Level) code** into a **GDSII** file, which is the final blueprint used by the fabrication plant (foundry) to create the masks for the integrated circuit. The entire flow is heavily guided by the **PDK (Process Design Kit)**, which provides all the necessary rules and components.

### 1. Synthesis (Synth)
* **Input:** RTL code (e.g., Verilog or VHDL).
* **Process:** Converts the behavioral RTL code into a **gate-level netlist**.
* **Component Source:** Uses components exclusively from the **Standard Cell Library (SCL)**, which contains pre-designed, characterized physical layouts for basic logic gates (like INV, NAND2, XOR2, DFF).
* **Output:** A list of standard cells and how they are logically connected.

### 2. Floor and Power Planning (FP+PP)
* **Goal:** Define the overall structure and power distribution of the chip area.
* **Macro Floor-Planning (FP):**
    * Determines the core **dimensions** (chip size).
    * Defines the placement and boundaries for internal memory blocks (macros) and the I/O **pin locations**.
    * Defines the **rows** where the standard cells will be placed.
* **Power Planning (PP):**
    * Creates the chip's power distribution network.
    * Involves laying down **Power Straps** (VDD and VSS tracks) and **Power Rings** around the core to ensure stable and even power delivery across the entire chip.

### 3. Placement (Place)
* **Goal:** Physically position every standard cell (gates and flip-flops) from the netlist onto the floorplan.
* **Process:** Places the cells onto the predefined **floorplan rows**, ensuring they are correctly **aligned with the sites** specified by the PDK.
* **Steps:** Usually performed in two steps:
    * **Global Placement:** Rough positioning of cells to minimize overall wire length.
    * **Detailed Placement:** Fine-tuning the position of each cell to ensure no overlap and to obey all design rules.

### 4. Clock Tree Synthesis (CTS)
* **Goal:** Create a robust network to deliver the clock signal to every sequential element (like flip-flops) on the chip.
* **Process:** Builds a **Clock Distribution Network** (usually in a tree-like structure, like an H-tree) from the clock input (CLK-in) to all elements.
* **Key Requirement:** The network must deliver the clock **with minimum skew** (the time difference between the clock arriving at different flip-flops), as zero skew is nearly impossible to achieve.

### 5. Routing (Route)
* **Goal:** Implement the physical wires (interconnects) that connect all the placed standard cells and macros, based on the netlist.
* **Process:** Uses the available **metal layers** defined in the PDK to draw the **metal tracks** on a huge **routing grid**.
* **Steps:** Performed in two stages:
    * **Global Routing:** Generates routing guides and estimates the path for long wires.
    * **Detailed Routing:** Implements the actual physical wiring using the guides, ensuring all wires respect the pitch, spacing, and width rules specified by the PDK.

### 6. Sign Off
* **Goal:** The final verification and validation stage to ensure the physical layout is completely ready for manufacturing.
* **Physical Verifications:** Checks if the design adheres to the factory's rules:
    * **DRC (Design Rule Check):** Verifies that the layout obeys all geometric and spacing rules from the PDK (e.g., no wires are too close or too thin).
    * **LVS (Layout Versus Schematic):** Checks that the physical layout (the drawn connections) exactly matches the original gate-level netlist (the intended circuit).
* **Timing Verification:** Checks if the chip will operate reliably at the required speed:
    * **STA (Static Timing Analysis):** Verifies that all signals arrive at their destination within the allocated clock cycle time (i.e., meets all performance requirements).
* **Final Output:** Once all checks pass, the final layout data is output in the **GDSII** format, which is sent to the foundry for mask creation and chip fabrication.

---
