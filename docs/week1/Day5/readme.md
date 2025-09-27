# Day5 : Optimization in synthesis

## Table of contents
-[If and Case Constructs](#introduction)

-[Lab 1 for Incomplete If statement](#Lab 1 for Incomplete If statement)

-[Lab 2 for Incomplete If statement](#Lab 2 for Incomplete If statement)

 

## If and Case Constructs

### 1. If–Else Construct

* Used to implement **priority logic**.
* Syntax:

  ```verilog
  if (cond1) 
      c1;
  else if (cond2) 
      c2;
  else if (cond3) 
      c3;
  else 
      e;
  ```
* Hardware mapping: cascaded multiplexers.
* **Priority**: only the first satisfied condition is executed.

### 2. Dangers with `if`

* If an **incomplete if** is written (without a final `else`), it may cause **inferred latches**.

  * Example:

    ```verilog
    if (cond1) y = a;
    else if (cond2) y = b;   // No else!
    ```
  * Hardware: latch inferred to hold value when no condition is true → BAD coding style.
* **Best practice:** Always include an `else` to cover all conditions.

### 3. Sequential Example (Counter)

```verilog
reg [2:0] count;
always @(posedge clk, posedge reset) begin
    if (reset)
        count <= 3'b000;
    else if (en)
        count <= count + 1;
end
```

* Here, if **enable is not high**, count retains its previous value (expected behavior).
* Correct usage of sequential `if`.


### 4. Case Statement

* Alternative to `if-else` for **multiplexing**.
* Example:

  ```verilog
  case (sel)
    2'b00: y = c1;
    2'b01: y = c2;
    2'b10: y = c3;
    2'b11: y = c4;
  endcase
  ```
* Hardware: single **n:1 multiplexer**.

### 5. Caveats with Case

1. **Incomplete Case → Inferred Latches**

   * Missing cases can leave outputs undefined.
   * Example:

     ```verilog
     case(sel)
       2'b00: y = c1;
       2'b01: y = c2;
     endcase
     ```
   * Fix: Always use `default`:

     ```verilog
     default: y = 0;
     ```

2. **Partial Assignments in Case**

   * If some signals are not assigned in all branches → results in latches.
   * Example:

     ```verilog
     case(sel)
       2'b00: begin x = a; y = b; end
       2'b01: begin x = c; end  // y not assigned!
       default: begin x = d; y = b; end
     endcase
     ```
   * Fix: Ensure **all outputs are assigned** in **all case branches**.

### 6. If vs Case

* **If–Else:**

  * Priority logic.
  * Only one branch executes.
* **Case:**

  * Parallel logic (all comparisons happen simultaneously).
  * Better for multiplexer-style selection.
  * Beware of **overlapping cases** → unpredictable outputs.


**Best Practices**

* Always cover all possibilities (else for if, default for case).
* Avoid partial assignments.
* Use if-else for priority logic, case for multiplexing.
* Avoid overlapping conditions in case.
  
---

## Lab 1 for Incomplete If statement

```
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
end
endmodule
```
#### Simulation using Iverilog

<img width="1503" height="490" alt="image" src="https://github.com/user-attachments/assets/7aefe955-1655-483f-9255-b036995a77c0" />

#### Synthesis using Yosys

<img width="1391" height="395" alt="image" src="https://github.com/user-attachments/assets/f00996a3-33c6-4b8c-81a4-101f8535cbe6" />

Incomplete if statement: not all input conditions assign a value to y.
In combinational logic (always @(*)), every output must be assigned for every input combination.
If not, the tool assumes storage is required → infers a level-sensitive latch.

--- 

## Lab 2 for Incomplete If statement

```
module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule
```
#### Simulation using Iverilog

<img width="1554" height="489" alt="image" src="https://github.com/user-attachments/assets/b849331f-e5e5-4672-bd1d-330b11ed702d" />

#### Synthesis using Yosys

<img width="1166" height="454" alt="image" src="https://github.com/user-attachments/assets/c6e47c97-bd3a-4061-b152-47a7feb8e16e" />

In combinational always @(*) blocks, all outputs must be assigned in every condition.
Here, there is no assignment when i0=0 and i2=0.
Synthesis compensates by creating a latch to hold the previous value of y.

If–Else If without Else is still incomplete → infers latch.
Simulation may look acceptable, but synthesis shows unintended hardware.
Always cover all conditions:
Use a final else.
Or provide a default assignment at the beginning of the block.

---

## Lab 3 (Incomplete case statement)

```
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	endcase
end
endmodule
```

#### Simulation using Iverilog

<img width="1457" height="483" alt="image" src="https://github.com/user-attachments/assets/e46b0afa-d6e4-4857-90a0-7bcdaba765f9" />

RTL Simulation (Icarus Verilog / GTKWave)
For sel = 2'b00 → y = i0.
For sel = 2'b01 → y = i1.
For sel = 2'b10 or 2'b11 → no assignment → y retains its previous value.
Retention of previous value indicates memory-like behavior.

#### Synthesis using Yosys

<img width="1624" height="515" alt="image" src="https://github.com/user-attachments/assets/1ea9d636-3715-4b0d-bb5d-4bfb7a8370cc" />

Synthesized circuit shows:
Multiplexer for sel=00 and sel=01.
For sel=10 or sel=11, tool infers latch to hold y.
Netlist confirms unintended latch insertion.

case statement did not cover all possible input combinations of sel (00, 01, 10, 11).
For uncovered values, y has no assignment.
To preserve simulation behavior, synthesis inserts a latch to hold the last value of y.

---

## Lab 4 (Complete case statement)

```
module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
		default : y = i2;
	endcase
end
endmodule
```

#### Simulation using Iverilog

<img width="1507" height="437" alt="image" src="https://github.com/user-attachments/assets/37331efd-2e63-46b7-994b-a91d5cd36712" />

For sel = 2'b00 → y = i0.
For sel = 2'b01 → y = i1.
For all other cases (sel = 2'b10 or 2'b11) → y = i2.
Output always has a defined value → no memory behavior.

#### Synthesis using Yosys

<img width="1624" height="515" alt="image" src="https://github.com/user-attachments/assets/ef1f6606-f387-4e10-a2ba-524cfc5b9050" />

Synthesized hardware: a multiplexer.
No latch is inferred since every possible sel value leads to an assignment for y.
Clean combinational logic.

All possible input combinations of sel are covered (00, 01, others via default).
Hence, y is always assigned for every sel.
This guarantees purely combinational hardware → no storage element needed.

---

## Lab 5 (Partial assignment case statement)

```
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule
```

#### Simulation using Iverilog

<img width="1425" height="541" alt="image" src="https://github.com/user-attachments/assets/401225d0-498a-4faa-9202-241a527974c5" />

RTL Simulation (Icarus Verilog / GTKWave)
sel = 00: y = i0, x = i2.
sel = 01: y = i1, but x is not assigned → simulation holds the old value of x.
sel = others: y = i2, x = i1.
Output shows memory-like behavior for x.



#### Synthesis using Yosys

<img width="1624" height="708" alt="image" src="https://github.com/user-attachments/assets/03c19e40-5abf-468e-b8b7-2c0e0a408a34" />

Synthesized circuit shows:
For y: always assigned in every case → pure combinational logic.
For x: not assigned when sel = 01 → synthesis adds latch for x to store previous value.
Netlist confirms latch inference for only one signal.

In combinational always @(*) blocks, every output signal must be assigned in every branch.
Here:
y → fully assigned in all cases.
x → missing assignment in one branch (sel = 01).
Result → Latch inferred only for x.

---

## Lab 6 (Overlapping condition in case statement)

```
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end

endmodule
```
#### Simulation using Iverilog

<img width="1694" height="525" alt="image" src="https://github.com/user-attachments/assets/b66e12d2-f294-42d7-84dc-2f8c94ac4cc5" />


#### Synthesis using Yosys

The GLS after synthesis:

<img width="1629" height="518" alt="image" src="https://github.com/user-attachments/assets/0a1b1702-d5c3-493c-bc83-3bc3689f5dd3" />


<img width="1362" height="424" alt="image" src="https://github.com/user-attachments/assets/f66f4409-a912-4b95-b1fe-48db5077b676" />


Even though the RTL had overlap (2'b10 and 2'b1?), the synthesis tool resolved it into a clean mux4 (no ambiguity in hardware).
This matches the priority behavior of the RTL, but the design intent can be misleading:
In RTL sim: 2’b10 matched i2 (first match priority).
In hardware: the tool mapped sel=10 → i2, sel=11 → i3.
So in this case, the hardware ended up consistent with simulation — but only because the tool picked the "expected" priority.

---

## For loop and For Generate constructs

Verilog supports loops mainly in two contexts:
1) for loop
2) generate for loop

###  for Loop

Purpose: Used for evaluating expressions repeatedly.

Context: Inside always blocks or procedural blocks.

Key Characteristics:

Executes at runtime during simulation.
Not meant for generating hardware instances.
Can contain blocking or non-blocking assignments.
Useful for repetitive logic calculations or sequential operations.

Example:
```
reg [3:0] sum;
integer i;

always @(*) begin
    sum = 0;
    for(i = 0; i < 4; i = i + 1) begin
        sum = sum + i;  // evaluates expression
    end
end
```

### generate For Loop

Purpose: Used for instantiating multiple hardware blocks (modules, wires, registers).

Context: Must be outside procedural blocks (always).

Key Characteristics:
Executes at compile-time (synthesizable).
Can instantiate multiple copies of hardware components.
Cannot contain regular procedural statements like blocking/non-blocking assignments directly.
Useful for replicating structures like buses, registers, or multiplexers.

Example : 
```
genvar i;
generate
    for(i = 0; i < 4; i = i + 1) begin : mux_block
        assign y[i] = sel[i] ? i1[i] : i0[i];  // generates 4 muxes
    end
endgenerate

```

| Feature               | `for` Loop                          | `generate for` Loop                                         |
| --------------------- | ----------------------------------- | ----------------------------------------------------------- |
| Execution             | Runtime (simulation)                | Compile-time (synthesis)                                    |
| Context               | Inside `always` or procedural block | Outside `always`                                            |
| Use                   | Expression evaluation               | Hardware instantiation                                      |
| Blocking/Non-blocking | Allowed                             | Not allowed                                                 |
| Example               | `for(i=0;i<4;i=i+1) y=y+i;`         | `for(genvar i=0;i<4;i=i+1) assign y[i]=sel[i]?i1[i]:i0[i];` |


Use Cases

for Loop:
Summing elements in an array.
Performing bitwise operations on buses.
Sequential calculations inside always blocks.

generate For Loop:
Instantiating multiple identical modules (e.g., 32-bit register file).
Creating arrays of multiplexers or flip-flops.
Replicating repetitive combinational logic like AND/OR gates.

---
## Labs for for loop , for generate

### Lab 1 - Mux using for loop 

```
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule
```

### Simulation using Iverilog 

<img width="1596" height="414" alt="image" src="https://github.com/user-attachments/assets/60bef68a-b4ee-445a-a2b6-2b088a4b01e0" />


### Synthesis using Yosys

<img width="1417" height="516" alt="image" src="https://github.com/user-attachments/assets/2b0be304-2c03-455f-8721-ae7f87cff334" />

Implemented 4:1 MUX in Verilog using a for loop.

Inputs combined into a 4-bit bus using concatenation {i3,i2,i1,i0}.

Loop checks k == sel → assigns correct input to output y.

Equivalent to standard MUX case/if statement.

Simulation (Icarus Verilog): verified correct functionality with waveform.

Synthesis (Yosys): design mapped to 4:1 multiplexer logic.

### Lab 2 - Demux using for loop 

```
module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule


```

### Simulation using Iverilog 

<img width="1759" height="464" alt="image" src="https://github.com/user-attachments/assets/7ec0db64-263e-4a43-8caa-ff90571d05f2" />


### Synthesis using Yosys

<img width="1098" height="621" alt="image" src="https://github.com/user-attachments/assets/47242544-c1f2-47fe-8a10-49369ba58769" />

Implemented 1-to-8 Demultiplexer in Verilog using a for loop.

Outputs grouped into an 8-bit register (y_int).

Loop iterates k=0 to 7, sets y_int[k]= i when k == sel.

All other outputs remain 0 (reset with y_int = 8'b0).

Equivalent to case-based demux design.

Simulation (Icarus Verilog): verified correct output routing.

Synthesis (Yosys): design mapped to 1:8 demux hardware.

Demonstrates use of for loop in always block for scalable demux design.

### Lab 3 - Ripple Carry Adder (RCA) using generate loop 

```
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

### Simulation using Iverilog 
<img width="1737" height="414" alt="image" src="https://github.com/user-attachments/assets/58acacf2-b84c-4bf3-8231-f94437e38127" />



### Synthesis using Yosys
<img width="1206" height="854" alt="image" src="https://github.com/user-attachments/assets/8f3ce65f-9b59-432c-834c-3b2362e79eda" />


Designed 8-bit Ripple Carry Adder (RCA) using generate loop.

Full Adder (fa) module is instantiated repeatedly for each bit.

Carry output of each stage connected to carry input of next stage.

First FA takes c=0, remaining FAs connected via generate loop.

Final carry (int_co[7]) becomes MSB of sum.

Simulation (Icarus Verilog): verified correct addition.

Synthesis (Yosys): design mapped to full-adder chain (RCA structure).

Demonstrates use of generate-for for scalable adder design.
