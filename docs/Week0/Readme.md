Task-1 : Create GitHub repo. Document summary of previous video(Getting Started with Digital VLSI SOC Design and planning) in the GitHub repo.

Summary of week0:

Chip modelling :
Write a Test bench in C language(The application that is going to be designed)

1) Test above application using GCC complier --> output is O0

2) Test the same application using specs GCC(eg: RISCV GCC) --> output is O1
If O0 = O1, Then our specifications are fixed.

3) Write softcopy of Hardware using RTL(Verilog) and Test with the application --> output is O2
If O1 = O2, Then our hardware softcopy is also fine.

Then, Design SoC
(i) Processor -> Gate level Netlist (synth)
(ii) Peripherals/IPs -> Macros(synth RTL)   and   -> Analog IPs (func RTL)

4) Test the application with the SoC Integration --> output is O3
Then O2 = O3
Then comes Floorplanning, Placement, Routing, Physical design
This combinely completes RTL to GDSII flow.
The GDSII is sent to factory(Tape out) after DRC/LVS checks.

5)After integration of physical chip with pheripherals, run the application --> output is O4
Then O0 = O1 = O2 = O3 = O4 
