

# Functional simulation of VSDBabySoC (Pre-synthesis simulation)

Functional simulation, or pre-synthesis simulation, is the first and most critical step in verifying any digital design. Its objective is to validate the design's logic and behavior against the specification, irrespective of timing delays or physical implementation.

For the VSDBabySoC, the goal is to confirm the high-level operation and data integrity among the main IP blocks: the RVMYTH processor, the PLL , and the DAC.

## PreSynthesis Simulation
### Steps 
#### Step 1: Clone VSDBabySoC - Top-Level SoC Module
```
git clone https://github.com/manili/VSDBabySoC.git
```
This repository contains the top-level SoC design named vsdbabysoc.
It integrates three main IP blocks:
RVMYTH — RISC-V core (processor),
AVSDPLL — Phase-Locked Loop (clock generator),
AVSDDAC — Digital-to-Analog Converter.

#### Step 2: Clone rvmyth - RISC-V
```
git clone https://github.com/kunalg123/rvmyth.git
```
This repository contains the RISC-V processor core (named rvmyth), designed in Verilog.
It serves as the CPU inside the VSDBabySoC.
The rvmyth core is responsible for instruction fetch, decode, execution, and memory operations.

#### Step 3: Clone avsdpll - PLL
```
git clone https://github.com/lakshmi-sathi/avsdpll_1v8.git
```
This repository provides the PLL (Phase-Locked Loop) module that generates a stable, high-frequency clock for the processor from a low-frequency input.

#### Step 4: Clone avsddac - DAC
```
git clone https://github.com/vsdip/rvmyth_avsddac_interface.git
```
This module provides the DAC interface that converts the digital output of the processor to an analog signal.
It helps visualize or analyze the analog behavior corresponding to digital computations within the SoC.


<img width="1854" height="890" alt="Screenshot from 2025-10-04 18-51-22" src="https://github.com/user-attachments/assets/3dac0d25-aef4-4267-822c-381864d988da" />


#### Step 5: Create Output Directory for Pre-Synthesis Simulation
```
cd VSDBabySoC
mkdir -p output/pre_synth_sim
```
A new folder named output/pre_synth_sim is created to store simulation results such as:
Compiled simulation files (.out),
Log files (.log),
Waveform files (.vcd).
Organizing the output into separate directories helps maintain a clean and structured project, especially when performing multiple simulations (pre-synthesis, post-synthesis, gate-level, etc.).


After setting up the directories and ensuring all module files (RVMYTH, PLL, DAC, SoC top, testbench) are available, the next steps involve compiling and running the simulation.

#### Step 6: Compile the Design using Icarus Verilog
```
iverilog -o /home/vamsi/VLSI/VSDBabySoC/output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -I /home/vamsi/VLSI/VSDBabySoC/src/include -I /home/vamsi/VLSI/VSDBabySoC/src/module /home/vamsi/VLSI/VSDBabySoC/src/module/testbench.v
```
This command compiles the Verilog design files and testbench using the Icarus Verilog simulator (iverilog).

-o	=> Specifies the output executable file name and path. In this case, it’s pre_synth_sim.out inside the output directory.

-DPRE_SYNTH_SIM	=> Defines a simulation macro (PRE_SYNTH_SIM) used inside the Verilog code to conditionally include/exclude certain parts meant for pre-synthesis simulation.

-I /src/include	 => Includes the directory containing Verilog header files (*.vh), like sp_verilog.vh.

-I /src/module	=> Includes the folder containing module definitions (like vsdbabysoc.v, rvmyth.v, etc.).

testbench.v	=> Specifies the top-level testbench that drives and monitors the SoC signals. It instantiates the vsdbabysoc module.

A compiled simulation executable: pre_synth_sim.out -> generated inside the /output/pre_synth_sim/ directory.

#### Step 7: Run the Simulation
```
cd output/pre_synth_sim
./pre_synth_sim.out
```
Executes the compiled simulation.
During execution:
The testbench stimulates the SoC (reset, clock, signals).
The simulation runs for a defined number of cycles.
A waveform file (pre_synth_sim.vcd) is generated automatically.

#### Step 8: Open Waveform in GTKWave
```
gtkwave pre_synth_sim.vcd
```

Open the generated .vcd waveform file in GTKWave — a graphical waveform viewer.
This allows you to analyze and verify:
Proper reset and clock behavior , 
Functional data flow between rvmyth, pll, and dac &
Correct SoC operation before synthesis.

<img width="1466" height="607" alt="image" src="https://github.com/user-attachments/assets/bdcec371-24d9-4c08-afba-fcd8566d4b87" />



