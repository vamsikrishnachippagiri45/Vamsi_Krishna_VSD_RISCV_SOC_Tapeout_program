# Static Timing Analysis (STA)

Static Timing Analysis (STA) is a method used to verify the timing performance of a digital circuit without applying input vectors. It checks whether all data paths meet the required timing constraints for correct operation at a given clock frequency. STA analyzes every possible timing path between start points (like flip-flop clock pins or input ports) and end points (like flip-flop data pins or output ports), ensuring that signals propagate within the allowed time limits.

Static Timing Analysis (STA) is a process used to verify that a digital circuit meets its timing requirements under all conditions, without running simulations. STA involves three main aspects — **timing checks, constraints, and library delay models**.

**Timing checks** ensure signals arrive at the correct time (like setup and hold checks).

**Constraints** define the required timing conditions, such as clock period, input/output delays, and margins.

**Library delay models** (like NLDM and Current Source models) describe how cell delays vary with load and input transition, enabling accurate timing prediction.

## Timing Checks

### 1. Concept of Timing Path

Every STA path has a start point (e.g., flip-flop clock pin or input port) and an end point (e.g., flip-flop data pin or output port).
The signal travels through various combinational logic elements, forming a timing path.
For each path, STA calculates:
Arrival time – actual signal arrival at the endpoint , 
Required time – expected time by which the signal must arrive,
Slack – difference between required and arrival time.

### 2. Setup Time Check

Ensures that data arrives before the clock edge that captures it.
Checks maximum delay paths (slow paths).
Setup violation occurs if arrival time > required time.
Affects the maximum operating frequency.

Setup slack = Required time – Arrival time
→ Negative slack = setup failure.

### 3. Hold Time Check

Ensures data remains stable after the capturing clock edge.
Checks minimum delay paths (fast paths).
Hold violation occurs if data changes too early after clock edge.

Hold slack = Arrival time – Required time
→ Negative slack = hold failure.

