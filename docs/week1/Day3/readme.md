# Day3 : Logic optimizations 
## Combinational logic optimization

Combinational Logic Optimization is reducing the logic to get a smaller, faster, and power-efficient design without changing functionality.

Why Optimize?
- Save area (fewer gates, fewer transistors), Save power (less switching, fewer transistors) & Improve timing (less delay).

Types of Optimizations

#### (a) Constant Propagation

If some inputs are fixed (0 or 1), the circuit can be simplified.

Example:

Y=((A⋅B)+C)′ => If A=0  → Y = (0+C)′ = C′

Result: big circuit reduced to just an inverter (from 6 MOSFETs → 2 MOSFETs).

#### (b) Boolean Logic Optimization

Use Boolean algebra/K-Map to simplify expressions.

Example:
assign y = a ? (b ? c : (c ? a : 0)) : (!c);

Simplifies step by step to:
Y=A⊕C
Reduces multiple multiplexers/gates to one XOR gate.

Methods Used:
Boolean simplification (Algebra, K-map, Quine McCluskey),
Constant propagation (remove unused logic),
Common sub-expression elimination (reuse repeated logic),
Gate-level replacement (replace with efficient gates, like XOR instead of MUX tree).

## Sequential Logic Optimization 

### Sequential Constant Propagation

If a flip-flop always produces a constant value (like always reset to 0 or always set to 1 regardless of clock), the downstream logic can be simplified.

Example:
If Q=0 always, then any logic driven by Q can be replaced by constant 0.
This reduces unnecessary flip-flops and gates. 

### Advanced Sequential Logic Optimizations

#### State Optimization

Reduces the number of states in a finite state machine (FSM).
Identifies equivalent states and merges them.
Leads to smaller, more efficient sequential circuits.

#### Retiming

Moves flip-flops across combinational logic without changing functionality.
Helps balance timing paths and meet clock frequency targets.
Example: shifting registers forward or backward to reduce critical path delay.

#### Sequential Logic Cloning (Floorplan-Aware Synthesis)

Duplicates sequential elements (flip-flops/latches) to improve placement and routing.
Useful in physical-aware synthesis to reduce interconnect delay and congestion.

## Labs for Optimization 
In Yosys, the command
```
opt_clean -purge
```
is an optimization pass. use this afer synthesis. Here’s what it does:

opt_clean - 
Removes unused or redundant objects from the netlist:
Unused wires
Unconnected cells
Dangling nets (signals not driving anything)
Basically, it cleans up the design after other optimization passes.

purge option -
Without -purge, Yosys only removes obviously unused wires/cells.
With -purge, Yosys becomes more aggressive:
Removes all unused wires and cells, even if they are publicly visible or marked for "keep" by default.
Cleans up constants or redundant flip-flops/latches that serve no purpose.

### Labs for combinational logic optimization
#### Lab 1 (opt_check.v):
```
module opt_check (input a , input b , input c , output y1, output y2);
wire a1;
assign y1 = a?b:0;
assign y2 = ~((a1 & b) | c);
assign a1 = 1'b0;
endmodule
```
After optimization the generated netlist is : 
<img width="1143" height="676" alt="image" src="https://github.com/user-attachments/assets/2fd54990-4f70-4951-aea1-f36efd71e3d9" />

#### Lab 2 (opt_check2.v):
```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```
After optimization the generated netlist is : 
<img width="1029" height="676" alt="image" src="https://github.com/user-attachments/assets/ab0a46d4-ebff-4bc1-995b-c25098ac9e77" />

#### Lab 3 (opt_check3.v)
```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
<img width="1004" height="676" alt="image" src="https://github.com/user-attachments/assets/051f675f-5f59-46fc-be15-b7755f65518e" />

#### Lab 4 (opt_check4.v)
```
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```
<img width="1004" height="676" alt="image" src="https://github.com/user-attachments/assets/d810f0a0-2a2a-4c74-a003-c2510441ba85" />


#### Lab 5 (multiple_module_opt.v)
```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 


endmodule
```
<img width="984" height="533" alt="image" src="https://github.com/user-attachments/assets/e86edd77-c3fa-4656-8cfe-135a845e1c5a" />


#### Lab 6 (multiple_module_opt2.v)
```
module sub_module(input a , input b , output y);
 assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
sub_module U2 (.a(b), .b(c) , .y(n2));
sub_module U3 (.a(n2), .b(d) , .y(n3));
sub_module U4 (.a(n3), .b(n1) , .y(y));

endmodule
```
<img width="1178" height="740" alt="image" src="https://github.com/user-attachments/assets/711a12f7-a763-4a1a-ba87-03c270357e17" />

### Labs for Sequential Logic Optimization 

#### Lab 7 ()
