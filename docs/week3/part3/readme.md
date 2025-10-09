
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

Netlist: **vsdbabysoc.synth.v** (The netlist file that got Post synthesis)

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
puts "Timing analysis complete."
exit
```



### OpenSTA TCL Script Explanation

| Command Block | Commands | Explanation |
| :--- | :--- | :--- |
| **1. Load Libraries** | `read_liberty -min ...` `read_liberty -max ...` | These commands load the **Liberty files (`.lib`)**, which contain the timing characteristics (delays, timing arcs) of all standard cells. Here we are loading three separate libraries (`sky130`, `avsddac`, `avsdpll`) and explicitly loading **both the minimum (`-min`) and maximum (`-max`) delay corners** for each. |
| **2. Load Design** | `read_verilog /.../vsdbabysoc.synth.v` | Loads the structural **synthesized netlist** of design, which defines how the standard cells are connected. |
| **3. Link Design** | `link_design vsdbabysoc` | This is a crucial step. It **resolves** the cell names found in the Verilog netlist by mapping them to the specific cell definitions and timing data stored in the Liberty files. This makes the design ready for timing analysis. |
| **4. Load Constraints** | `read_sdc /.../vsdbabysoc_synthesis.sdc` | Loads the **Synopsys Design Constraints (SDC)** file. This file tells the STA tool the timing requirements: clock frequency, clock source, input/output delays, and any timing exceptions (like false paths). |
| **5. Initial Check** | `report_checks` | Runs a general summary check, often looking for basic issues like unconstrained inputs, unclocked registers, or timing loops. |


### Timing Analysis and Reporting Commands

These commands execute the core STA checks and save the output.

| Command | Path Target | Purpose & Output |
| :--- | :--- | :--- |
| **Detailed Setup Report** | `report_checks -path_delay max -format full -group_path_count 1 -sort_by_slack > .../setup_critical_path.txt` | **Generates the critical path report for Setup (Max Delay).** **`-path_delay max`**: Focuses on the slowest paths. **`-format full`**: Provides the detailed path breakdown (the required "graph" of cell and net delays). **`-group_path_count 1`**: Restricts the report to only the single worst path. |
| **Detailed Hold Report** | `report_checks -path_delay min -format full -group_path_count 1 -sort_by_slack > .../hold_critical_path.txt` | **Generates the critical path report for Hold (Min Delay).** **`-path_delay min`**: Focuses on the fastest paths. This check ensures data doesn't arrive too early relative to the clock edge. |
| **WNS Metric** | `report_wns -max > .../wns_setup.txt` | Calculates and reports the **Worst Negative Slack (WNS)** for setup checks. This is the absolute worst timing violation (or best margin) in your entire design. |


## Run OpenSTA

```
cd ~/VLSI/OpenSTA/build
sta ../examples/BabySOC/run_sta.tcl
```

<img width="1576" height="519" alt="image" src="https://github.com/user-attachments/assets/85e49c98-b738-4248-b991-5b521731b75a" />


<img width="906" height="849" alt="image" src="https://github.com/user-attachments/assets/0021cdf8-7d79-49f1-a18a-0cf18c11a0ee" />



## Output files 

### 1) reports/setup_critical_path.txt → Setup critical path

```
Startpoint: _9169_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: _8516_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ _9169_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.29    0.29 v _9169_/Q (sky130_fd_sc_hd__dfxtp_1)
   5.22    5.51 ^ _4026_/Y (sky130_fd_sc_hd__nor4_1)
   0.68    6.19 v _4159_/Y (sky130_fd_sc_hd__a31oi_1)
   0.73    6.92 v _5444_/X (sky130_fd_sc_hd__or3_1)
   4.32   11.24 ^ _6241_/Y (sky130_fd_sc_hd__nor4_1)
   0.38   11.61 v _6306_/Y (sky130_fd_sc_hd__a311oi_1)
   0.00   11.61 v _8516_/D (sky130_fd_sc_hd__dfxtp_1)
          11.61   data arrival time

  13.00   13.00   clock clk (rise edge)
   0.00   13.00   clock network delay (ideal)
   0.00   13.00   clock reconvergence pessimism
          13.00 ^ _8516_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.58   12.42   library setup time
          12.42   data required time
---------------------------------------------------------
          12.42   data required time
         -11.61   data arrival time
---------------------------------------------------------
           0.81   slack (MET)

```

### 2) reports/hold_critical_path.txt → Hold critical path

```
Startpoint: _8411_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: _9157_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ _8411_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.27    0.27 ^ _8411_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.27 ^ _9157_/D (sky130_fd_sc_hd__dfxtp_1)
           0.27   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00   clock reconvergence pessimism
           0.00 ^ _9157_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.03   -0.03   library hold time
          -0.03   data required time
---------------------------------------------------------
          -0.03   data required time
          -0.27   data arrival time
---------------------------------------------------------
           0.31   slack (MET)

```



