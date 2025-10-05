

# Functional simulation of VSDBabySoC (Pre-synthesis simulation)

Functional simulation, or pre-synthesis simulation, is the first and most critical step in verifying any digital design. Its objective is to validate the design's logic and behavior against the specification, irrespective of timing delays or physical implementation.

For the VSDBabySoC, the goal is to confirm the high-level operation and data integrity among the main IP blocks: the RVMYTH processor, the PLL , and the DAC.

## PreSynthesis Simulation
### Steps 
Step 1: Clone VSDBabySoC - Top-Level SoC Module
```
git clone https://github.com/manili/VSDBabySoC.git
```
This repository contains the top-level SoC design named vsdbabysoc.
It integrates three main IP blocks:
RVMYTH — RISC-V core (processor),
AVSDPLL — Phase-Locked Loop (clock generator),
AVSDDAC — Digital-to-Analog Converter.

Step 2: Clone rvmyth - RISC-V
```
git clone https://github.com/kunalg123/rvmyth.git
```
This repository contains the RISC-V processor core (named rvmyth), designed in Verilog.
It serves as the CPU inside the VSDBabySoC.
The rvmyth core is responsible for instruction fetch, decode, execution, and memory operations.

Step 3: Clone avsdpll - PLL
```
git clone https://github.com/lakshmi-sathi/avsdpll_1v8.git
```
This repository provides the PLL (Phase-Locked Loop) module that generates a stable, high-frequency clock for the processor from a low-frequency input.

Step 4: Clone avsddac - DAC
```
git clone https://github.com/vsdip/rvmyth_avsddac_interface.git
```
This module provides the DAC interface that converts the digital output of the processor to an analog signal.
It helps visualize or analyze the analog behavior corresponding to digital computations within the SoC.

Step 5: Create Output Directory for Pre-Synthesis Simulation
```
cd VSDBabySoC
mkdir -p output/pre_synth_sim
```
A new folder named output/pre_synth_sim is created to store simulation results such as:
Compiled simulation files (.out),
Log files (.log),
Waveform files (.vcd).
Organizing the output into separate directories helps maintain a clean and structured project, especially when performing multiple simulations (pre-synthesis, post-synthesis, gate-level, etc.).






