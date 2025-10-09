
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

Netlist: vsdbabysoc.synth.v  and 
Constraints: vsdbabysoc_synthesis.sdc

Timing Libraries:
sky130_fd_sc_hd__tt_025C_1v80.lib , 
avsdpll.lib , 
avsddac.lib

Netlist & Constraints are placed under ~/VLSI/OpenSTA/examples/BabySOC/ and timing libraries under ~/VLSI/OpenSTA/examples/timing_libs/.











