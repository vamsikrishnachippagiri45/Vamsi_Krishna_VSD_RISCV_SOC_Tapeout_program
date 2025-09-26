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
In hierarchical synthesis, a design is divided into multiple modules (sub-blocks).
Each module is synthesized separately and then combined into the top-level netlist.

This helps in:
Easier design management.
Reusability of modules.
Better optimization by the tool across modules.

```
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

The synthesized design of the multiple modules (It shows the heirarchical modules only)
<img width="1064" height="676" alt="image" src="https://github.com/user-attachments/assets/db180981-e043-40ce-85f1-0d0e2b8fb1cb" />

The netlist of the multiple modules : 
<img width="1854" height="891" alt="image" src="https://github.com/user-attachments/assets/32d8d176-bd06-449b-bc78-22be273968fb" />
In CMOS logic, both NAND and NOR can implement the same logic in different forms (due to De Morgan’s laws).
But the tool generally prefers NAND implementations over NOR. PMOS transistors are weaker (low mobility) compared to NMOS.

In NOR gates, PMOS are stacked in parallel, but NMOS are stacked in series.
In NAND gates, NMOS are stacked in series, but PMOS are stacked in parallel.

Problem:
Stacked PMOS (like in NOR) → very slow pull-up → requires larger PMOS width → more area + power.
Stacked NMOS (like in NAND) → not a big problem, because NMOS mobility is much higher.

Therefore, NAND-based implementations are more efficient in CMOS technology.

To flatten the netlist use 
```
flatten
```
The flattened netlist can be seen (No heirarchical modules are present)
<img width="920" height="683" alt="image" src="https://github.com/user-attachments/assets/d82d914a-cb4e-4e53-994e-fce9f0ea479e" />
And the mutiple module after flattening 
<img width="1854" height="891" alt="image" src="https://github.com/user-attachments/assets/b9efd505-596b-41b8-a2f6-a67e542da9a6" />

We can synthesis the submodules also:

To synthesize only a specific submodule:
```
synth -top submodule1
```
Why Use Submodule Synthesis?

Efficiency (Reuse of Netlist):
If the same submodule is instantiated multiple times in the top-level design,
Synthesizing it once → generating a netlist → reusing it saves time and effort.

Divide and Conquer:
In large designs (e.g., CPUs, SoCs), synthesizing everything at once is complex.
Breaking into smaller submodules → synthesize individually → combine later.
Makes debugging, timing closure, and optimization easier.
