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

## Lab for Incomplete If statement

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

