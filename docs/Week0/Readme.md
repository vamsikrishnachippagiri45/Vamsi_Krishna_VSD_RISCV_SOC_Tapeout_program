***Task-1 : Create GitHub repo. Document summary of previous video(Getting Started with Digital VLSI SOC Design and planning) in the GitHub repo.***

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

4) Test the application with the SoC Integration --> output is O3. 
Then O2 = O3.
Then comes Floorplanning, Placement, Routing, Physical design. 
This combinely completes RTL to GDSII flow.
The GDSII is sent to factory(Tape out) after DRC/LVS checks.

5) After integration of physical chip with pheripherals, run the application --> output is O4. 
Then O0 = O1 = O2 = O3 = O4.



***Task-2 : Install tools listed in the document using the machine configuration mentioned. Update
your GitHub repo with Tool snapshot.***

**System Check**

6GB RAM, 50 GB HDD

Ubuntu 20.04+

4vCPU

**Tool check**

**YOSYS**


```bash
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it)
$ sudo apt-get install build-essential clang bison flex \
libreadline-dev gawk tcl-dev libffi-dev git \
graphviz xdot pkg-config python3 libboost-system-dev \
libboost-python-dev libboost-filesystem-dev zlib1g-dev
#Yosys depends on a GitHub submodule called **abc**, which must be initialized before building. If not, you may see errors during compilation. 
$ git submodule update --init --recursive
$ make config-gcc
$ make
$ sudo make install
```
<img width="1436" height="827" alt="Screenshot from 2025-09-19 10-21-19" src="https://github.com/user-attachments/assets/21ad404c-2b18-4063-a461-7c6235a83b24" />




