# Chip Floor-Planning Considerations

---

## 1) IC Area and Geometry (Utilization Factor and Aspect Ratio)

### i. Core vs. Die Section of a Chip

The chip is physically divided into two main areas:

* **Die:** The **entire piece of semiconductor material (silicon)** cut from the wafer.
    * It contains everything, including the **Core**, **I/O pads**, and any surrounding passive components.
* **Core:** The central area of the die dedicated to **active logic circuits**.
    * This is where the **Standard Cells** (like the D Flip-Flops (FF) and logic gates (A1, O1) shown in the diagram) and macros are placed and connected.
    * The core is typically where the automated **Place and Route (PnR)** process occurs.

The diagram illustrates how a small logic circuit (DFFs connected by combinational logic) is conceptually converted into physical units occupying area within the core.


### ii. Utilization Factor

The Utilization Factor is a critical metric in floor planning and placement, indicating how densely the core area is populated with actual logic cells.

$$\text{Utilization Factor} = \frac{\text{Area Occupied by Netlist (Total Cell Area)}}{\text{Total Area of the Core}}$$

* **Netlist Area:** The sum of the physical area of all standard cells (gates, flip-flops, etc.) used in the design.
* **Core Area:** The total available area within the core boundaries for placing those cells.
* **Purpose:** The utilization factor is usually set to a value **less than 1** (e.g., 0.65 to 0.85). This remaining empty space (often $\approx 15\% - 35\%$) is necessary for:
    * **Routing:** Making space for the wires (interconnects).
    * **Decoupling Capacitors/Filler Cells:** Inserting components for power stability and to fill empty spaces.
    * **Managing Congestion:** Preventing the design from becoming too crowded, which would make routing impossible or lead to timing failures.


### iii. Aspect Ratio

The Aspect Ratio defines the shape of the core area.

$$\text{Aspect Ratio} = \frac{\text{Height}}{\text{Width}}$$

* **Purpose:** Designers choose the aspect ratio during **Floor Planning** to determine the core's shape.
    * An Aspect Ratio of **1** results in a square core.
    * An Aspect Ratio **greater than 1** results in a taller, rectangular core.
    * An Aspect Ratio **less than 1** results in a wider, rectangular core.
* **Impact:** The chosen aspect ratio can influence routing complexity, clock distribution, and the overall physical design flow.

---


## 2) Pre-placed Cells (Macros)

**Pre-placed cells** are large, complex functional blocks within a chip that have user-defined locations. They are positioned early in the design flow, before the main automated placement process begins.

* **Examples of Pre-placed Cells/IPs:**
    * **Memory** blocks (SRAM, ROM).
    * **Clock-gating cells** (for power management).
    * **Comparators** and other complex analog or mixed-signal blocks.
    * **Mux** (Multiplexer) or other large custom logic blocks.
* **Key Characteristic:** These blocks are often referred to as **Macros** or Intellectual Properties (**IPs**). Unlike small standard cells (like an inverter or a NAND gate), their size, function, and connection requirements demand that their locations be fixed manually or semi-manually by the designer.

Floorplanning determines the location and orientation of the **Pre-placed Cells (Macros)** relative to the **Core** boundaries and to each other.
Because these large blocks cannot be moved easily by the automated tools, they are placed first. The **automated placement-and-routing tools** then take over, placing the **remaining logical cells** (the thousands of small standard gates) into the empty spaces around these pre-placed cells.

<img width="908" height="626" alt="image" src="https://github.com/user-attachments/assets/d7b64741-3c84-4db8-8ca8-f55ef1c1f5ba" />


---


## 3) Decoupling Capacitors 

Decoupling Capacitors (or **Decaps**, represented as $C_d$) are essential components in IC design used to maintain a stable power supply voltage within the circuit.

### The Problem: Power Supply Noise (Ground Bounce)

In any complex digital circuit, the power supply line ($\text{Vdd}$) and ground line ($\text{Vss}$) are not ideal. They contain parasitic resistance ($R_{dd}$, $R_{ss}$) and parasitic inductance ($L_{dd}$, $L_{ss}$) due to the physical wiring and bonding.

* **Switching Current Demand ($I_{\text{peak}}$):** When a circuit (like a processor core) switches its logic states, it demands a large surge of current called the **peak switching current ($I_{\text{peak}}$)** in a very short time.
* **Voltage Drop:** Due to the parasitic resistance and inductance in the power lines, this large, fast-changing current causes an instantaneous **voltage drop** across the parasitics.
    * The voltage at the circuit's internal power node ($\text{Vdd}'$) is momentarily reduced from the external supply ($\text{Vdd}$).
    * $V_{\text{drop}} \approx I_{\text{peak}} \times (R_{\text{dd}} + R_{\text{ss}}) + L_{\text{eff}} \times \frac{dI}{dt}$
* **Consequence:** If $\text{Vdd}'$ momentarily drops below the required **noise margin**, a logic '1' signal output by the circuit might be seen as a low (or unknown) signal at the input of the next circuit, leading to **functional failure** and incorrect operation.


### The Solution: Decoupling Capacitors

Decoupling Capacitors are added in **parallel** with the circuit's power supply lines ($\text{Vdd}$ and $\text{Vss}$) to mitigate this voltage fluctuation.

* **Function as a Charge Reservoir:** The Decap acts as a **local charge reservoir**.
* **During Switching:** When the circuit demands a sudden surge of **peak current ($I_{\text{peak}}$)**, the current is instantaneously drawn from the nearby $\mathbf{C_d}$, rather than from the distant external power supply through the noisy **RL network** ($R_{dd}$, $L_{dd}$). This keeps the local $\text{Vdd}'$ stable.
* **Replenishing Charge:** After the switching event, the slower **RL network** is used to replenish the charge back into the $C_d$ for the next cycle.

<img width="635" height="491" alt="image" src="https://github.com/user-attachments/assets/f4cf815f-3be5-4c75-8de8-c68897456fad" />

---

## 4) Power Planning

Power Planning is the process of designing the on-chip power distribution network to ensure a stable supply voltage ($\text{Vdd}$) and ground ($\text{Vss}$) to all active logic components.


### The Problem: Simultaneous Switching Noise (SSN)

When many circuits switch their state simultaneously (e.g., when a large data bus changes its value), they create sudden, large current demands, which causes noise in the power lines.

* **Scenario:** Consider a 16-bit bus changing its value, where some bits switch from $\text{'0'}$ to $\text{'V'}$ (charge) and others switch from $\text{'V'}$ to $\text{'0'}$ (discharge).
* **Voltage Droop:** All circuits switching from $\text{'0'}$ to $\text{'V'}$ (charging) demand current simultaneously. This large current surge flows through the parasitic inductance and resistance ($\text{Ldd}$, $\text{Rdd}$) of the single $\text{Vdd}$ tap point. This causes the on-chip $\text{Vdd}$ to momentarily **drop** or "droop."
* **Ground Bounce:** Similarly, all circuits switching from $\text{'V'}$ to $\text{'0'}$ (discharging) dump current simultaneously into the single $\text{Vss}$ tap point. This current surge flows through the parasitic $\text{Lss}$ and $\text{Rss}$, causing the on-chip ground ($\text{Vss}$) to momentarily "bounce" above the ideal $\text{0V}$ reference.
* **Consequence:** Both droop and bounce reduce the effective voltage supplied to the logic cells, potentially leading to incorrect logic operation and circuit failure.


### The Solution: Power Grid and Decaps

To counteract SSN, a uniform and low-impedance power distribution network is implemented.

* **Power Grid:** The ideal solution is a **mesh-like grid** structure across the entire core area.
    * It consists of overlapping horizontal and vertical lines of **$\text{Vdd}$ (blue)** and **$\text{Vss}$ (green)**, forming a checkerboard pattern.
    * This grid drastically lowers the effective parasitic resistance and inductance by providing **multiple, highly redundant paths** for current to flow, minimizing localized voltage drops.
* **Decoupling Capacitors (Decaps):** As discussed previously, $\mathbf{C_d}$ are added at every logic block (driver/load) to act as a **local reservoir** of charge. When a gate switches, it draws current from the closest $\mathbf{C_d}$ rather than the distant external power supply. 

### Power Planning Implementation

The final power plan integrates the power grid with pre-placed cells:

* **Grid Structure:** The $\text{Vdd}$ and $\text{Vss}$ lines are overlaid across the entire core, typically using the thicker, lower-resistance metal layers (M3, M4, etc.) for better current delivery. * **Pre-placed Cells:** Large blocks ($\text{Block a}$, $\text{Block b}$, etc.) are placed first, and surrounding them, **Decoupling Capacitors ($\text{DECAP}1, \text{DECAP}2, \text{DECAP}3$)** are inserted. These capacitors ensure the macro blocks, which often draw the highest peak current, have an immediate and stable power source.

The Power Planning step is essential for **signal integrity** and **power integrity**, guaranteeing reliable circuit operation at the intended speed.

<img width="616" height="489" alt="image" src="https://github.com/user-attachments/assets/62235f78-c3de-4791-b9f2-c847ea428153" />

---


These slides illustrate the final steps of the **Floorplanning** stage, focusing on **I/O Pin/Pad Placement** and preparing the core area for the subsequent **Placement & Routing** steps.

***

## 5) Pin Placement and Cell Blockage

The final steps of Floorplanning ensure the chip's core is properly defined with power delivery, external connections, and reserved areas for large blocks, making it ready for the automated Place and Route (PnR) tools.

###  I/O Pin/Pad Placement

This step defines where the external signals enter and exit the chip's core.

* **Definition:** **I/O Pins/Pads** are the physical interfaces located at the edge of the **Die** that connect the chip to the outside world (like the package pins).
* **Placement Strategy:** The location of these pads ($\text{Din}1$ to $\text{Din}4$, $\text{Dout}1$ to $\text{Dout}4$, $\text{CLK}1$, $\text{CLK}2$, $\text{ClkOut}$) is determined by system-level requirements and signal integrity needs.
    * In the image, the pads are strategically placed around the core perimeter, connecting the external world to the internal power grid and logic.
    * **$\text{Din/Dout}$ Pads:** Handle the data signals defined by the **netlist** (the VHDL/Verilog code that describes the connectivity between the internal Flip-Flops (FFs) and logic gates).
    * **$\text{CLK}$ Pads:** Handle the clock signals, which are vital for system timing.
* **Ready State:** Once pins are placed, the power grid ($\text{Vdd}$ and $\text{Vss}$ mesh) is considered complete across the core, extending to the pads.

###  Logical Cell Placement Blockage

This step reserves space in the core area to prevent the automated tools from placing small logic gates in specific regions.

* **Blockage Definition:** Large **pre-placed cells** (Macros) like $\text{Block a}$, $\text{Block b}$, and $\text{Block c}$ cannot have standard logic cells placed on top of them. Additionally, areas surrounding these blocks or areas reserved for specific routing needs must be marked as blocked.
* **Role of Decaps:** Decoupling capacitors ($\text{DEC P1, DEC P2, DEC P3}$) are inserted adjacent to these Macros to stabilize their power supply. These $\text{DEC}$ cells also take up space and must be accounted for.
* **Goal:** By defining these areas as **Placement Blockages** (the gray area in the final diagram), the tools know the exact available area (the empty rows) where they must place the remaining thousands of standard cells (the FFs and logic gates from the netlist).

**Final State:** Once the pins are placed, the power grid is laid, and the placement blockages are defined, the **Floor Plan is Ready for the Placement & Routing Step** in the main ASIC flow. 

<img width="606" height="486" alt="image" src="https://github.com/user-attachments/assets/aa6f225c-8b3c-45b7-970c-f22d33b8eaaa" />

---
