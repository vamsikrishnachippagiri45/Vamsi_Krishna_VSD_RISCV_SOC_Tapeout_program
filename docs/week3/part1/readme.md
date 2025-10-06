# Gate-Level Simulation (GLS) of VSDBabySoC

Gate-Level Simulation (GLS) is performed after synthesis to verify that the gate-level netlist behaves functionally the same as the RTL design.
It uses the synthesized Verilog netlist, which is mapped to standard cells from the technology library (Sky130).

Unlike functional (RTL) simulation, GLS includes:
Actual gate delays (optional via #delay or SDF).
Real standard cells (from .lib or .v models).
Verification that synthesis did not alter functionality.

GLS ensures logical equivalence between the RTL design and the synthesized hardware before physical design.

---

## Synthesis with Yosys

Goal: Convert RTL (behavioral Verilog) → Gate-level netlist using standard cells from Sky130 library.

### Open Yosys

```
yosys
```

### Read the top-level RTL design

```
read_verilog /home/vamsi/VLSI/VSDBabySoC/src/module/vsdbabysoc.v
```

This command loads the top module of your SoC (vsdbabysoc.v) into Yosys.
It defines the main SoC structure — connecting the RVMYTH processor, PLL, and DAC.

### Read the RVMYTH processor RTL file

```
read_verilog -I /home/vamsi/VLSI/VSDBabySoC/src/include /home/vamsi/VLSI/VSDBabySoC/src/module/rvmyth.v
```

Loads the RVMYTH processor module and includes the directory that contains header files (.vh) or macros using the -I option.
This ensures all include statements inside the RTL are correctly resolved.

### Read the Sky130 standard cell library

```
read_liberty -lib /home/vamsi/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Loads the Sky130 HD (High Density) standard cell library at typical process corner (TT), 25°C, and 1.8 V.
This file describes all standard cells (INV, NAND, NOR, DFF, etc.) that Yosys can use to build your gate-level design.


### Read the DAC macro library

```
read_liberty -lib /home/vamsi/VLSI/VSDBabySoC/src/lib/avsddac.lib
```
Loads the DAC macro library.
This allows Yosys to understand the timing and interface of the analog DAC used in your SoC.


<img width="1920" height="922" alt="image" src="https://github.com/user-attachments/assets/b06705b0-dd8b-4465-93d6-a933592e8b30" />


### Read the PLL macro library

```
read_liberty -lib /home/vamsi/VLSI/VSDBabySoC/src/lib/avsdpll.lib
```
Loads the PLL library.
This provides Yosys with the functional and timing details of the PLL macro block integrated in the SoC

### Perform synthesis

```
synth -top vsdbabysoc
```
Performs RTL-to-gate-level synthesis for the top-level module vsdbabysoc.
It converts behavioral RTL (written with always blocks, assign, etc.) into a netlist made of generic gates.

<img width="1836" height="475" alt="image" src="https://github.com/user-attachments/assets/a6755af3-6d71-4373-bc6d-5967d4efa4b7" />


### Map flip-flops to technology cells

```
dfflibmap -liberty /home/vamsi/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Maps all flip-flops (always @(posedge clk) registers) to actual DFF cells from the Sky130 library.
Ensures every register is replaced by a real physical cell available in the technology.

### Optimize the logic

```
 opt
```
Performs logic optimization — removes redundant gates, merges equivalent logic, simplifies expressions, and reduces area or power while keeping functionality identical.

### Technology mapping using ABC

```
abc -liberty /home/vamsi/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Runs the ABC tool to map all logic gates to real standard cells from the .lib file.
ABC performs both optimization and mapping, transforming abstract logic into technology-specific gates.

### Flatten hierarchy

```
flatten
```

Combines all submodules into one flat netlist.
After flattening, Yosys creates a single-level design, which simplifies simulation and analysis.

### Replace undefined states

```
setundef -zero
```
Replaces all undefined (x) or high-impedance (z) signals with logic 0.
This prevents unknown or unpredictable behavior during simulation.

### Clean the design

```
clean -purge
```

Removes unused wires, cells, and nets left after optimization.
Keeps the netlist tidy and minimal.


### Rename all nets systematically

```
rename -enumerate
```

Renames internal nets, modules, and instances to a consistent numbered format (U1, U2, etc.).
This makes the final synthesized netlist more readable and easier to debug.

### Display synthesis statistics

```
stat
```

Prints statistics such as:
Total number of cells and flip-flops ,
Number of logic levels. 
Useful for checking synthesis quality.

<img width="1356" height="915" alt="image" src="https://github.com/user-attachments/assets/7b16d153-8d98-4ebf-b4f8-30e0cb8c2ead" />


<img width="1356" height="915" alt="image" src="https://github.com/user-attachments/assets/d6da0238-8499-4f43-a7a6-65d4377250b0" />


###  Write the synthesized netlist

```
write_verilog -noattr /home/vamsi/VLSI/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v
```

Writes the final synthesized gate-level netlist to a Verilog file.
This file (vsdbabysoc.synth.v) will be used for post-synthesis (GLS) simulation.

---

## Post-Synthesis Simulation (GLS)

Goal: Verify the post-synthesis netlist behaves identically to the RTL design.

### Compile the design and testbench

```
iverilog -o /home/vamsi/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out \
-DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 \
-I /home/vamsi/VLSI/VSDBabySoC/src/include \
-I /home/vamsi/VLSI/VSDBabySoC/src/module \
-I /home/vamsi/VLSI/VSDBabySoC/output/post_synth_sim \
-I /home/vamsi/VLSI/VSDBabySoC/src/gls_model \
/home/vamsi/VLSI/VSDBabySoC/src/module/testbench.v

```

Compiles the testbench and synthesized netlist for GLS using iverilog.

| Flag               | Meaning                                                                         |
| ------------------ | ------------------------------------------------------------------------------- |
| `-o`               | Output file name (`post_synth_sim.out`).                                        |
| `-DPOST_SYNTH_SIM` | Macro for conditional compilation (used to switch between RTL and GLS).         |
| `-DFUNCTIONAL`     | Uses **functional models** of standard cells (without transistor-level delays). |
| `-DUNIT_DELAY=#1`  | Assigns a **unit delay (#1)** to each gate for simple timing behavior.          |
| `-I`               | Add include paths for header files, models, and the synthesized netlist.        |
| `testbench.v`      | Top-level testbench to simulate the design.                                     |


### Run the simulation

```
cd /home/vamsi/VLSI/VSDBabySoC/output/post_synth_sim
./post_synth_sim.out
```

Executes the compiled simulation binary.
This will generate:
VCD waveform file (post_synth_sim.vcd) for waveform analysis.


### Open the waveform

```
gtkwave post_synth_sim.vcd
```

Launches GTKWave, a waveform viewer, to visualize signal transitions, check clock/reset, and verify that the post-synth design behaves the same as the RTL simulation.


<img width="1636" height="475" alt="Post_synthesis_simulation_GLS" src="https://github.com/user-attachments/assets/58af3f6e-651a-4a3b-81f1-abf4b7983f13" />

---

