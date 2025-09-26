# Day2 : Library file, hierarchical Vs flat synthesis and Efficient flop coding styles
## Introduction to library files
Meaning of sky130_fd_sc_hd__tt_025C_1v80.lib

sky130_fd_sc_hd → Process and library name (SkyWater 130nm, standard cells, high density).

tt → Process corner = Typical-Typical.

025C → Temperature = 25°C.

1v80 → Voltage = 1.80 V.

This means the library models how the cells behave in typical process, at 25°C temperature, and 1.8V supply voltage.

##### Cells are affected by three variations:

##### Process (P) → Variations during manufacturing.

Example: Transistors may come out slightly faster or slower due to fabrication differences.
Corners: ff (fast-fast), ss (slow-slow), tt (typical-typical).

##### Voltage (V) → Power supply can vary.

Example: 1.8V nominal, but may drop to 1.62V or rise to 1.98V.
Affects delay (lower V = slower).

##### Temperature (T) → Chips heat up or cool down.

Example: -40°C (cold, fast) vs 125°C (hot, slow).

Libraries are provided for different PVT corners → so your silicon works in all conditions.

CMOS technology cells leak power depending on input state.
Larger cells = more area → higher leakage.
Smaller cells = less area → lower leakage but slower.

## Hierarchical design 

<img width="1064" height="676" alt="image" src="https://github.com/user-attachments/assets/db180981-e043-40ce-85f1-0d0e2b8fb1cb" />


