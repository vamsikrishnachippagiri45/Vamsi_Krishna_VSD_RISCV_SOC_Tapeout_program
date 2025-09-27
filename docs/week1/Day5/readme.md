# Optimization in synthesis

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
