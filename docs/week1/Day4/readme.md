# Day4 : GLS (Gate level simulation), Sybthesis-Simulation Mismatch , Blocking and Non-blocking statements

## Table of Contents
- [Day4 : GLS (Gate Level Simulation), Synthesis-Simulation Mismatch, Blocking and Non-blocking statements](#day4--gls-gate-level-simulation-synthesis-simulation-mismatch-blocking-and-non-blocking-statements)
  - [What is GLS (Gate Level Simulation)](#what-is-gls-gate-level-simulation)
  - [Why GLS](#why-gls)
  - [GLS Flow (with Icarus Verilog & GTKWave)](#gls-flow-with-icarus-verilog--gtkwave)
  - [RTL → Netlist → Gate Models](#rtl--netlist--gate-models)
  - [Synthesis-Simulation Mismatches](#synthesis-simulation-mismatches)
    - [Missing sensitivity list](#missing-sensitivity-list)
    - [Blocking vs Non-blocking misuse](#blocking-vs-non-blocking-misuse)
    - [Non-standard Verilog coding styles](#non-standard-verilog-coding-styles)
  - [Missing Sensitivity List Example](#missing-sensitivity-list-example)
  - [Blocking vs Non-Blocking Assignments](#blocking-vs-non-blocking-assignments)
    - [Sequential logic caveats](#sequential-logic-caveats)
    - [Combinational logic caveats](#combinational-logic-caveats)
  - [Key Notes](#key-notes)
  - [Labs for GLS](#labs-for-gls)
    - [Lab 1: Ternary operator mux](#lab-1-ternary-operator-mux)
    - [Lab 2: Synthesis-simulation mismatch due to sensitivity list](#lab-2-synthesis-simulation-mismatch-due-to-sensitivity-list)
    - [Lab 3: Synthesis-simulation mismatch due to blocking statements](#lab-3-synthesis-simulation-mismatch-due-to-blocking-statements)
  - [Conclusion](#conclusion)


## What is GLS (Gate Level Simulation)

* GLS = Running testbench on **post-synthesis netlist** (instead of RTL).
* Netlist = logically same as RTL, but expressed as **primitive gates**.
* Same testbench works (if RTL was coded properly).

### Why GLS

* To verify **logical correctness** of design after synthesis.
* To ensure **timing is met** (requires delay-annotated gate models).
* Helps catch issues not visible in RTL simulation.

### GLS Flow (with Icarus Verilog & GTKWave)

1. **Inputs:**

   * Synthesized **netlist** (gate-level Verilog).
   * **Testbench**.
   * **Standard cell library models** (gate-level models).
2. Run with **iverilog** → produces executable + VCD (Value Change Dump).
3. View waveforms in **gtkwave**.
4. If models have **delays**, GLS also checks **timing**.

### 3. RTL → Netlist → Gate Models

Example:
RTL:

```verilog
assign y = (a & b) | c;
```

Netlist:

```verilog
and u1 (.a(a), .b(b), .y(n1));
or  u2 (.a(n1), .b(c), .y(y));
```

* Gate models are **functional** (AND, OR, etc.).
* With delay annotation → **timing-aware**.
* GLS checks both **functionality + timing**.

---

## Synthesis-Simulation Mismatches

Causes of mismatch between RTL sim vs. GLS sim:

1. **Missing sensitivity list**

   * Using `always @(sel)` in combinational logic misses inputs.
   * Correct form: `always @(*)` (sensitive to all inputs).
2. **Blocking (`=`) vs Non-Blocking (`<=`) misuse**

   * Wrong use can cause mismatch between intended hardware vs. simulation.
3. **Non-standard Verilog coding styles**

   * E.g., latch inference, mixed blocking assignments.



## Missing Sensitivity List Example

```verilog
// Wrong
always @(sel) begin
   if(sel) y = i1;
   else    y = i0;
end

// Correct
always @(*) begin
   if(sel) y = i1;
   else    y = i0;
end
```

* `@(*)` = automatic sensitivity to all RHS signals.
* Prevents mismatches between RTL sim and synthesized logic.


## Blocking vs Non-Blocking Assignments

* Inside `always` block:

  * `=` (blocking): sequential execution (line by line).
  * `<=` (non-blocking): parallel execution (all RHS evaluated first).
* **Guideline:**

  * Use **blocking (`=`)** for **combinational always blocks**.
  * Use **non-blocking (`<=`)** for **sequential (clocked) always blocks**.


### Caveats with Blocking Assignments

### Example 1 (Sequential Logic):

```verilog
always @(posedge clk) begin
   q  = q0;   // uses updated q0 immediately
   q0 = d;
end
```

* Hardware intended = two flip-flops.
* Simulation = one flop because of blocking order.
* Fix: use non-blocking (`<=`).

```verilog
always @(posedge clk) begin
   q  <= q0;
   q0 <= d;
end
```

### Example 2 (Combinational Logic):

```verilog
always @(*) begin
   y  = q0 & c;   // uses old value of q0
   q0 = a | b;
end
```

* Simulation uses **stale q0**.
* Synthesizer rearranges → mismatch.
* Correct order: update q0 first OR use separate always block.

---

## Key Notes

* GLS = essential step after synthesis to validate logic & timing.
* Common mismatches arise from **coding mistakes**:

  * Incomplete sensitivity lists.
  * Wrong use of blocking vs non-blocking.
  * Poor coding styles.
* Always follow Verilog best practices:

  * `always @(*)` for combinational.
  * `<=` for sequential.
  * Clear, synthesizable code only.


## Labs for GLS
### Lab 1 (Ternary operator - mux)
```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
```
#### i) RTL simulation using Iveriog
<img width="1470" height="493" alt="image" src="https://github.com/user-attachments/assets/71facf70-f854-4bc1-98ff-d84a94328b95" />

#### ii) GLS using Iverilog

First we have synthesize the design using yosys to get the netlist and then we have to give the netlist to Iverilog

The Netlist after synthesis is : 
<img width="992" height="649" alt="image" src="https://github.com/user-attachments/assets/8bfef131-d46e-4a45-8b21-07e4c12d03eb" />

To check whether the synthesized netlist (gate-level design) behaves the same as your RTL design, using real standard-cell models from the Sky130 library.

Inputs to the command:

primitives.v → defines basic logic primitives (AND, OR, etc.).

sky130_fd_sc_hd.v → functional models of Sky130 standard cells.

ternary_operator_mux_net.v → your synthesized gate-level netlist.

tb_ternary_operator_mux.v → your testbench (stimulus + waveform dump).

Together, they allow GLS to run and verify correctness of the synthesized design.

```
iverilog /home/vamsi/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v 
        /home/vamsi/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v 
        ternary_operator_mux_net.v tb_ternary_operator_mux.v
```
<img width="1474" height="609" alt="image" src="https://github.com/user-attachments/assets/2ac12005-9901-49ac-abb4-4d3ede3445b9" />

### Lab 2 (Synthesis-Simulation Mismatch due to sensitivity list- bad_mux.v)

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

#### i) RTL simulation using Iveriog

<img width="1408" height="474" alt="image" src="https://github.com/user-attachments/assets/93f22e17-e909-4ea2-bd24-b487cf6a9268" />

#### ii) GLS using Iverilog

The Netlist is : 

<img width="960" height="547" alt="image" src="https://github.com/user-attachments/assets/f29b28fc-e1eb-43cb-977f-5471644a8906" />

After simulating with Netlist : 

<img width="1553" height="455" alt="image" src="https://github.com/user-attachments/assets/ba73c488-afb1-4564-a349-93debb9544e9" />

We can see that there is mismatch in the simulation and synthesis of the bad_mux.v
Using GLS it used correct mux , but the code we wrote does not specify the mux. 

Issue:
Sensitivity list is incomplete (always @(sel) only).

In RTL simulation:
Block triggers only when sel changes.
If i0 or i1 change while sel stays constant, y is not updated → wrong behavior.

In Synthesis:
Tool infers a proper mux cell (sky130_fd_sc_hd__mux2_1).
Real mux reacts to all inputs (i0, i1, sel).
Hardware works correctly.

RTL simulation can lie if sensitivity lists are incomplete.
Synthesis + GLS reflect the real hardware.

### Lab 3 (Synthesis-Simulation Mismatch due to Blocking Statements)

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```

#### i) RTL simulation using Iveriog

<img width="1583" height="454" alt="image" src="https://github.com/user-attachments/assets/3b521202-3aed-48b6-825b-6aac52115854" />


#### ii) GLS using Iverilog

The Netlist is : 
<img width="936" height="632" alt="image" src="https://github.com/user-attachments/assets/10b1e34c-2dda-4592-8951-f87c62600455" />

After simulating with Netlist : 
<img width="1342" height="503" alt="image" src="https://github.com/user-attachments/assets/0a5c8445-91f1-4330-b7c4-854ec8c40aed" />

Behavior in RTL Simulation : 
Simulation executes statements line by line (blocking =).
d = x & c; is computed using the previous value of x.
Then x is updated (x = a | b;).
Result: d lags one step behind expected value. Results in Wrong functional behavior.

Behavior in Synthesis / GLS :
Synthesizer analyzes intent, not line order.
It sees that d depends on a, b, c.
Netlist built as:
x = a | b;
d = (a | b) & c;
Hardware muxes and gates ensure correct dependency. GLS produces correct result.

Note : Reorder assignments so variables are updated before use. or Use blocking (=) in combinational blocks with care. Use non-blocking (<=) in sequential (clocked) blocks.

## Conclusion 

Gate Level Simulation (GLS) is a crucial step to verify that synthesized netlists function correctly and meet timing requirements, capturing issues that may not appear in RTL simulation. Common mismatches arise from incomplete sensitivity lists, improper use of blocking versus non-blocking assignments, and non-standard coding practices. Writing clear, synthesizable Verilog—using always @(*) with blocking for combinational logic and <= for sequential logic—ensures consistency between simulation and hardware. GLS labs demonstrate that RTL simulations can sometimes mislead due to coding mistakes, while GLS reflects the true hardware behavior, highlighting the importance of adhering to best coding practices.
