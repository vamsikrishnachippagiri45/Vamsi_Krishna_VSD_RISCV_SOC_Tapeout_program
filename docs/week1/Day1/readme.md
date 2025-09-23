# Day1 : Introduction to Verilog RTL Design and synthesis

## 1) Simulator, Design and Testbench 
RTL (Register Transfer Level) design is checked for adherence to the specification by simulating the design.
A simulator tool is used to execute and verify the behavior of Verilog code before moving to synthesis.

We use Icarus Verilog (iverilog) simulator.

### Design 
A Design is a set of Verilog RTL codes written to implement the intended functionality.
It describes the digital circuit behavior at register-transfer level.

Example: A multiplexer, adder, counter, etc.


### Testbench 
A Testbench is a separate Verilog code used to verify the design.
It does not represent actual hardware; instead, it: 1) Provides stimulus (inputs) to the design. 2) Monitors and checks the outputs of the design.

Testbenches help in ensuring that the design meets the functional specification.

<img width="1419" height="674" alt="Screenshot from 2025-09-23 10-20-14" src="https://github.com/user-attachments/assets/fa540de8-5932-428b-8f22-812afc53d76a" />


### How simulator work (Iverilog based simulation flow): 

The simulator mimics how the digital circuit behaves over time. It: Reads the design code (DUT – Design Under Test), Reads the testbench code (stimulus + checks).

Compiles both into a simulation executable.
Runs the simulation and generates output (waveforms, logs, or results).

we use gtkwave for viewing output waveforms. 

<img width="1536" height="693" alt="Screenshot from 2025-09-23 10-20-46" src="https://github.com/user-attachments/assets/b388fa91-9c12-422d-bd93-4fc1467ee494" />

## 2) Lab using Iverilog and gtkwave (simulation of 2x1 mux)

### (i) clone the repository into a folder.
``` 
sudo -i

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
This git contains all the verilog files and libraries. 
The DC_WORKSHOP contain the lib folder which contain standard cell library and verilog files folder which contains verilog files and testbenches.
<img width="1796" height="893" alt="image" src="https://github.com/user-attachments/assets/7c8dc666-c735-4512-a619-5eb6989656a8" />

### (ii) Load design and testbench in iverilog
```
iverilog good_mux.v tb_good_mux.v
```
It creates a.out file. Execute that a.out file. Then It is going to dump it into vcd file. 
```
./a.out
```
Now open gtkwave to see the output waveform by giving this vcd file as input to gtkwave. 
```
gtkwave tb_good_mux.vcd
```

<img width="1836" height="871" alt="image" src="https://github.com/user-attachments/assets/25ff4f1d-9b67-4e08-9546-1b3fdeb7312c" />
<img width="1843" height="462" alt="image" src="https://github.com/user-attachments/assets/c3e1e68f-7475-4879-9a0a-63009bc58bb1" />

### (iii) Analyse the verilog code 
To open both the design file and the testbench file side by side using gvim, use:
```
gvim tb_good_mux.v -o good_mux.v 
```
Installing GVim (if not installed) : 
If gvim is not already installed on your system, install it using:
```
sudo apt update
sudo apt install vim-gtk3 -y
```
This will install GVim (graphical Vim editor), which provides a more user-friendly interface for editing Verilog files compared to the terminal-only vim.

<img width="1439" height="853" alt="image" src="https://github.com/user-attachments/assets/1061f4d3-10f4-4cb9-aa8b-d51471c7609d" />

## Introduction to Yosys 
Yosys is an open-source RTL synthesis tool.
It takes a Verilog RTL design and a standard cell library (.lib), then produces a gate-level netlist.

Inputs to Yosys: 

Design (RTL in Verilog):
The user-written code describing the circuit functionality.
command : read_verilog (Loads the RTL design files into Yosys.)

Technology Library (.lib file):
A file containing timing, area, and power information of standard cells (like NAND, NOR, DFF, etc.).
command : read_liberty
(Loads the standard cell library (.lib).)

Output from Yosys:

A gate-level netlist (e.g., good_mux_netlist.v).
Netlist only contains standard cells (from .lib).
command : write_verilog
(Writes out the synthesized netlist in Verilog format.)

synthesis:

Performs generic synthesis of the design (logic optimizations, conversions).
command : synth -top 
<img width="1114" height="602" alt="Screenshot from 2025-09-23 13-26-32" src="https://github.com/user-attachments/assets/073fda49-5df8-43e7-8ebe-6fd71c605ddf" />

#### Verifying the design 

Once synthesis is completed in Yosys, we obtain a gate-level netlist. But synthesis is only useful if we can confirm that the netlist behaves the same as the original RTL design. This is done using simulation after synthesis.

1. Inputs for Verification

Netlist File (Gate-level Verilog)-
Produced by Yosys after synthesis.
Contains only standard cells from the .lib file.

Testbench-
Same testbench that was used for RTL simulation.
Applies the same input stimulus to the netlist design and checks the outputs.

2. Simulation using Icarus Verilog (iverilog)

Both the testbench and the netlist are compiled together using iverilog.
The simulator executes the design behavior cycle-by-cycle.
During simulation, the design signals are dumped into a VCD (Value Change Dump) file.

3. VCD File

.vcd file is an output waveform file generated during simulation.
It records changes of signals over time.
Example: inputs (a, b, sel) and output (y) of a mux.

4. GTKWave (Waveform Viewer)

GTKWave is used to view the VCD file in graphical form.
Designers can visually check whether the output waveforms match the expected behavior.
If RTL simulation and gate-level simulation produce the same waveforms, synthesis is considered successful.
<img width="1074" height="490" alt="image" src="https://github.com/user-attachments/assets/92b0c140-893e-47f7-a865-d5ed01acdb63" />

