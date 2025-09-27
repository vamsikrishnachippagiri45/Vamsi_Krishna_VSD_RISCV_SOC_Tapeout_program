# Day4 : GLS (Gate level simulation), Sybthesis-Simulation Mismatch , Blocking and Non-blocking statements

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

### Lab 2 (Synthesis-Simulation Mismatch - bad_mux.v)

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
