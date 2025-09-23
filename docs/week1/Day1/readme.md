# Day1 : Introduction to Verilog RTL Design and synthesis

## 1) Simulator, Design and Testbench 
RTL (Register Transfer Level) design is checked for adherence to the specification by simulating the design.
A simulator tool is used to execute and verify the behavior of Verilog code before moving to synthesis.

We use Icarus Verilog (iverilog) simulator.

#### Design 
A Design is a set of Verilog RTL codes written to implement the intended functionality.
It describes the digital circuit behavior at register-transfer level.

Example: A multiplexer, adder, counter, etc.


#### Testbench : A Testbench is a separate Verilog code used to verify the design.
It does not represent actual hardware; instead, it: 1) Provides stimulus (inputs) to the design. 2) Monitors and checks the outputs of the design.

Testbenches help in ensuring that the design meets the functional specification.

<img width="1419" height="674" alt="Screenshot from 2025-09-23 10-20-14" src="https://github.com/user-attachments/assets/fa540de8-5932-428b-8f22-812afc53d76a" />


#### How simulator work (Iverilog based simulation flow): 

The simulator mimics how the digital circuit behaves over time. It: Reads the design code (DUT – Design Under Test), Reads the testbench code (stimulus + checks).

Compiles both into a simulation executable.
Runs the simulation and generates output (waveforms, logs, or results).

we use gtkwave for viewing output waveforms. 

<img width="1536" height="693" alt="Screenshot from 2025-09-23 10-20-46" src="https://github.com/user-attachments/assets/b388fa91-9c12-422d-bd93-4fc1467ee494" />

## 2) Lab using Iverilog and gtkwave (simulation of 2x1 mux)

#### step1 : clone the repository into a folder.
''' 
sudo -i

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
'''
This git contains all the verilog files and libraries. 
The DC_WORKSHOP contain the lib folder which contain library and verilog files folder which contains verilog files and testbenches.
<img width="1796" height="893" alt="image" src="https://github.com/user-attachments/assets/7c8dc666-c735-4512-a619-5eb6989656a8" />


