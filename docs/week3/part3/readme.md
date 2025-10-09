
# Timing Analysis using OpenSTA

## OpenSTA Setup

Installed dependencies for OpenSTA build:
```
sudo apt-get update
sudo apt-get install -y cmake gcc g++ tcl-dev tcl-tclreadline libeigen3-dev swig bison flex automake
```

CUDD library installed for STA:
```
git clone https://github.com/ivmai/cudd.git
cd cudd
./configure --prefix=/usr/local
make -j$(nproc)
sudo make install
```

OpenSTA Build:
```
cd ~/VLSI/OpenSTA
mkdir -p build
cd build
cmake ..
make -j$(nproc)
sudo make install
```

Check STA binary:
```
which sta
sta
```

<img width="996" height="365" alt="image" src="https://github.com/user-attachments/assets/3b8aae42-f510-4d3d-8a2a-2508e9d3cce4" />


## OpenSTA Input Files

File Organization : 

Netlist: **vsdbabysoc.synth.v**  and 
Constraints: **vsdbabysoc_synthesis.sdc**

Timing Libraries:
**sky130_fd_sc_hd__tt_025C_1v80.lib**  , 
**avsdpll.lib** , 
**avsddac.lib**.

Netlist & Constraints are placed under ~/VLSI/OpenSTA/examples/BabySOC/ and timing libraries under ~/VLSI/OpenSTA/examples/timing_libs/.

And a TCL script file (run_sta.tcl) is created to give input to the OpenSTA.

<img width="756" height="343" alt="image" src="https://github.com/user-attachments/assets/838b8d4a-3f1b-4d05-8bb4-30dde82bd18b" />


## OpenSTA TCL Script

OpenSTA is an EDA (Electronic Design Automation) tool designed to perform complex Static Timing Analysis. Like many command-line EDA tools (e.g., those for synthesis, place-and-route), it uses TCL (Tool Command Language) as its scripting and command interface. 

TCL script sets up the entire Static Timing Analysis (STA) environment for the vsdbabysoc design, performs the analysis, and generates the necessary reports.


```
read_liberty -min /home/vamsi/VLSI/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max /home/vamsi/VLSI/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min /home/vamsi/VLSI/OpenSTA/examples/timing_libs/avsddac.lib
read_liberty -max /home/vamsi/VLSI/OpenSTA/examples/timing_libs/avsddac.lib
read_liberty -min /home/vamsi/VLSI/OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty -max /home/vamsi/VLSI/OpenSTA/examples/timing_libs/avsdpll.lib
read_verilog /home/vamsi/VLSI/OpenSTA/examples/BabySOC/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc /home/vamsi/VLSI/OpenSTA/examples/BabySOC/vsdbabysoc_synthesis.sdc
report_checks
report_checks -path_delay max -format full -group_path_count 1 -sort_by_slack > /home/vamsi/VLSI/OpenSTA/reports/setup_critical_path.txt
report_checks -path_delay min -format full -group_path_count 1 -sort_by_slack > /home/vamsi/VLSI/OpenSTA/reports/hold_critical_path.txt
report_wns -max > /home/vamsi/VLSI/OpenSTA/reports/wns_setup.txt
report_tns -max > /home/vamsi/VLSI/OpenSTA/reports/tns_setup.txt
puts "Timing analysis complete."
exit
```





