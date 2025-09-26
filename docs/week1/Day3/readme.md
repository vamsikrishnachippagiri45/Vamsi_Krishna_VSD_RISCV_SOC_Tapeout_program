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

## Lab for Optimization 
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



