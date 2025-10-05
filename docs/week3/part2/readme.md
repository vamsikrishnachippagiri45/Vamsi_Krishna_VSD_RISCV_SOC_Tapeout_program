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

## Types of setup/hold Analysis

### reg2reg (Register to Register)
This is the most fundamental path in synchronous design, analyzing the delay from the Q output of the Launch Flop to the D input of the Capture Flop through combinational logic. It's the primary determinant for the circuit's maximum operating frequency and is checked for both setup (maximum delay) and hold (minimum delay).

### in2reg (Input to Register)
This path ensures that external data arriving at an Input Port (IN) reaches the D pin of the Launch Flop in time to be reliably captured. The required arrival time is defined by the set_input_delay constraint, which models the external board and off-chip component delays.

### reg2out (Register to Output)
This path checks the delay from the Q pin of the Capture Flop to an Output Port (OUT). It verifies that the data remains stable at the output for the duration required by external components, using the set_output_delay constraint to define the external timing requirements.

### in2out (Input to Output)
This path analyzes the timing through any section of the circuit that is purely combinational, starting at an Input Port and ending at an Output Port. Since no clock edge is involved in launching or capturing, it's typically constrained using set_max_delay and set_min_delay to enforce maximum and minimum allowable latency.

### clock gating

This check focuses on the logic used to enable or disable the clock signal for a sequential element (e.g., an AND gate controlled by a gating_signal). It reduces on-chip power.

### recovery/removal

These checks are performed on asynchronous control pins like RST (Reset) or Set. Recovery is a setup check ensuring the asynchronous signal is stable before the clock edge, while removal is a hold check ensuring stability immediately after the clock edge, thereby preventing the sequential element from entering a metastable state.

### data-to-data (Data-to-Data Timing)

This refers to timing paths between two asynchronous pins or two data pins where there is no common clock for launching and capturing. The typical check is between two non-clock data pins of a sequential element, such as an internal path delay.

### Latch (Time Borrow / Time Given)

A latch is a level-sensitive storage element that allows data to pass when the clock is active.
Unlike flip-flops, latches enable time borrowing, meaning if a signal arrives late in one phase, it can still be captured during the next active phase — effectively borrowing time from the next stage.
This helps relax setup time requirements for slow paths.
However, if a latch borrows time, the next stage must give time, meaning it has less time to complete its own operation.
Hence, latch-based designs balance time borrow and time given across pipeline stages to improve overall timing performance.
