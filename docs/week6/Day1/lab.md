<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/64ebdb3c-235b-4ce6-b8b3-5b0d20d10040" />

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/7ff35c90-c452-457d-b09e-65e97e3e3ac0" />

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/2f4d3a71-90a7-466e-a8ff-651c2ffd4a8b" />

<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/13ffe00e-6b44-48a4-9fff-d02ce2063c49" />

<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/c3f05c56-78a2-4aa6-8982-64f79023e872" />



# Lab : Synthesis of picorv32a Core

### I. Environment Setup and Tool Flow

The lab successfully sets up and utilizes the **OpenLANE** flow, a powerful open-source toolchain used for digital **ASIC (Application-Specific Integrated Circuit)** design.

1.  **Environment Setup:** The design environment was configured within a Linux Virtual Machine (VM), leveraging the **Oracle VM VirtualBox** to create a **VDI (Virtual Desktop Infrastructure)**. The user environment (`vsduse`r) was verified.


<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/5363e6d2-db29-4fa0-a22b-42ab9400ab77" />

2.  **Tool Flow Initialization:** The OpenLANE directory was navigated, confirming the presence of necessary design files (like `picorv32a`) and PDK data (e.g., `sky130A`). The flow was executed using the `flow.tcl -interactive` command.



3.  **Target Design:** The target design for this experiment is **`picorv32a`**, a widely used open-source **RISC-V 32-bit CPU core** designed in **RTL (Register Transfer Level)**.



### II. Synthesis Run and Configuration

The synthesis phase converts the high-level behavioral RTL code into a gate-level netlist using components from the **Standard Cell Library (SCL)**, which is provided by the **SkyWater 130nm PDK**.

1.  **Synthesis Command:** The synthesis step was executed and verified, leading to the generation of various reports. 
2.  **Verilog Backend:** The synthesis tool (**Yosys**) successfully processed the `picorv32a` Verilog code, mapped the logic to the `sky130_fd_sc_hd` standard cell library, and completed the mapping.
3.  **Output Analysis:** The output reports directory (`reports/synthesis`) was inspected to access the final cell statistics and timing reports. 

### III. Synthesis Results and Flop Ratio Calculation

The synthesis report provides a statistical breakdown of the components used to implement the design. This allows for the calculation of the **Flop Ratio**, which indicates the proportion of sequential logic (memory elements) to the total logic.

#### A. Data Extraction

From the `picorv32a` synthesis statistics report: 

* **Total Number of Cells:** $18,036$ (This includes all logic gates and sequential elements).
* **Number of D Flip-Flops (DFFs):** $1,613$ (This represents the sequential storage elements).

#### B. Calculation

The Flop Ratio is calculated as follows:

$$\text{Flop Ratio} = \frac{\text{Number of D Flip Flops}}{\text{Total Number of Cells}}$$

$$\text{Flop Ratio} = \frac{1,613}{18,036} \approx 0.0894$$

The Percentage of DFFs is:

$$\text{Percentage of DFFs} = \text{Flop Ratio} \times 100$$

$$\text{Percentage of DFFs} = 0.0894 \times 100 \approx \mathbf{8.94\%}$$

### IV. Conclusion

The synthesis of the `picorv32a` core using the OpenLANE flow was successful. The resulting netlist has a **Flop Ratio of approximately 0.0894** or **8.94%**. This ratio signifies that about 8.94% of the logic utilized for the CPU core is dedicated to sequential memory (registers and flip-flops), while the remaining portion is dedicated to combinational logic (gates). This is a typical distribution for a processor core, as a significant portion of the cell count is dedicated to the combinational pathways that perform arithmetic and control operations.
