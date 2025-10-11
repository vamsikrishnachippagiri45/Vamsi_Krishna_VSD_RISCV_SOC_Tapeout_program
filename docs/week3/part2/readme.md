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

---

##  Timing Checks Analysis

### Types of setup/hold Analysis

#### 1) reg2reg (Register to Register)
This is the most fundamental path in synchronous design, analyzing the delay from the Q output of the Launch Flop to the D input of the Capture Flop through combinational logic. It's the primary determinant for the circuit's maximum operating frequency and is checked for both setup (maximum delay) and hold (minimum delay).

#### 2) in2reg (Input to Register)
This path ensures that external data arriving at an Input Port (IN) reaches the D pin of the Launch Flop in time to be reliably captured. The required arrival time is defined by the set_input_delay constraint, which models the external board and off-chip component delays.

#### 3) reg2out (Register to Output)
This path checks the delay from the Q pin of the Capture Flop to an Output Port (OUT). It verifies that the data remains stable at the output for the duration required by external components, using the set_output_delay constraint to define the external timing requirements.

#### 4) in2out (Input to Output)
This path analyzes the timing through any section of the circuit that is purely combinational, starting at an Input Port and ending at an Output Port. Since no clock edge is involved in launching or capturing, it's typically constrained using set_max_delay and set_min_delay to enforce maximum and minimum allowable latency.

#### 5) clock gating

This check focuses on the logic used to enable or disable the clock signal for a sequential element (e.g., an AND gate controlled by a gating_signal). It reduces on-chip power.

#### 6) recovery/removal

These checks are performed on asynchronous control pins like RST (Reset) or Set. Recovery is a setup check ensuring the asynchronous signal is stable before the clock edge, while removal is a hold check ensuring stability immediately after the clock edge, thereby preventing the sequential element from entering a metastable state.

#### 7) data-to-data (Data-to-Data Timing)

This refers to timing paths between two asynchronous pins or two data pins where there is no common clock for launching and capturing. The typical check is between two non-clock data pins of a sequential element, such as an internal path delay.

#### 8) Latch (Time Borrow / Time Given)

A latch is a level-sensitive storage element that allows data to pass when the clock is active.
Unlike flip-flops, latches enable time borrowing, meaning if a signal arrives late in one phase, it can still be captured during the next active phase — effectively borrowing time from the next stage.
This helps relax setup time requirements for slow paths.
However, if a latch borrows time, the next stage must give time, meaning it has less time to complete its own operation.
Hence, latch-based designs balance time borrow and time given across pipeline stages to improve overall timing performance.


Static Timing Analysis (STA) performs several supporting analyses to ensure reliable signal transitions, proper loading, and a stable clock network. These checks help maintain timing accuracy and circuit integrity.



### Slew/Transition Analysis
This analysis verifies the rise and fall times of signals to ensure transitions are neither too slow nor too fast.

Data Slew (max/min):
Max slew (slow transition) → increases delay → possible setup violations.
Min slew (too fast) → may cause unrealistic timing optimism.

Clock Slew (max/min):
Slow clock transitions degrade both setup and hold margins, affecting flip-flop triggering accuracy.



### Load Analysis

Ensures each gate drives an appropriate load for correct timing and signal quality.

Fanout (max/min):
Max fanout → overloaded net → high capacitance → slow slew.
Min fanout → ensures efficient loading for signal integrity.

Capacitance (max/min):
Checks total load capacitance on each net.
Excess capacitance increases delay and causes signal distortion.



### Clock Analysis

Evaluates the clock network quality, since all STA checks depend on accurate clock timing.

Clock Skew:
Difference between capture and launch clock arrival times.
Directly impacts setup and hold margins.

Pulse Width:
Ensures the clock high and low durations meet the minimum width requirement.
Violations can cause metastability or missed clocking events.

---

## Timing Paths and Graph-Based Timing Analysis in STA

### Timing Paths (Combinational Circuit: Reg-to-Reg)

A timing path represents the flow of a signal from one register (flip-flop) to another through combinational logic.
In STA, this path is analyzed to ensure data launched by one register is correctly captured by the next within the clock period.
A typical register-to-register path consists of :
Launch Register  :  Source flip-flop where data starts.
Combinational Logic  :  Intermediate logic gates causing delay.
Capture Register  :  Destination flip-flop where data is captured.
Path Delay  :  Sum of cell delays and interconnect delays between launch and capture.
Timing paths are checked for both setup (maximum delay) and hold (minimum delay) requirements.


### Timing Graphs (Directed Acyclic Graph – DAG)

In STA, the circuit is represented as a Directed Acyclic Graph (DAG) for efficient computation of arrival and required times.
Nodes: Represent pins of logic gates or registers.
Edges: Represent signal propagation (delays) between pins.
The graph is acyclic since timing flows from inputs to outputs without loops.
This graph model helps tools efficiently calculate path delays, slack, and critical paths.


### Logic Gates to Nodes

Each logic gate pin (input/output) is converted into a node in the timing graph.
This allows detailed tracking of timing for each connection and ensures precise delay propagation from one gate to another.

### Actual Arrival Time (AAT)

The actual arrival time is when the signal actually reaches a specific node or pin in the circuit.
Calculated by accumulating delays along the path from the start point.

### Required Arrival Time (RAT)

The required arrival time is the latest (for setup) or earliest (for hold) time by which a signal must arrive at a node for correct operation.
Derived from clock period and constraints.
Used as a timing reference to compare against AAT.

### Slack

Slack = Required Arrival Time – Actual Arrival Time.
Indicates the timing margin of a path.
Positive Slack: Timing met.
Negative Slack: Timing violated.


### Graph-Based Analysis (GBA)

GBA calculates timing for all paths simultaneously using the DAG model.
Fast and efficient but sometimes overly pessimistic, as it assumes the worst-case conditions for every path.
Used in early design stages for quick timing closure estimation.


### Path-Based Analysis (PBA)

PBA focuses on specific critical paths identified from GBA.
It re-evaluates the exact path delays more accurately by removing pessimistic assumptions.
Provides realistic slack values, improving correlation with post-layout timing.


### Converting Pins to Nodes for Complete Slack Analysis

In detailed STA, each input and output pin of a gate is represented as a node to track the exact timing through every transition.

---

## Setup Time, Hold Time, Clock-to-Q Delay, Clock Skew and Jitter

| **Parameter**                                       | **Definition**                                                                          | **Effect on Timing**                                                   | **Timing Equation (Including Skew & Jitter)**                                                                   | **Remarks / Notes**                                           |
| --------------------------------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Setup Time (T<sub>setup</sub>)**                  | Minimum time before the **clock edge** that data must be stable at the flip-flop input. | If violated → **Setup violation**, data not captured correctly.        | T<sub>cq</sub> + T<sub>comb</sub>  ≤ T<sub>clk</sub>  - T<sub>setup</sub> + T<sub>skew</sub> − T<sub>jitter</sub> | Reduces **maximum clock frequency**; affects performance.     |
| **Hold Time (T<sub>hold</sub>)**                    | Minimum time after the **clock edge** that data must remain stable.                     | If violated → **Hold violation**, old data may be overwritten.         | T<sub>cq</sub> + T<sub>comb</sub> ≥ T<sub>hold</sub> + T<sub>skew</sub> + T<sub>jitter</sub>                    | Affects data stability; fixed by adding delay in short paths. |
| **Clock-to-Q Delay (T<sub>cq</sub>)**               | Time from the **clock edge** to when output (Q) changes after a valid clock.            | Contributes to total data path delay.                                  | Used in both setup and hold equations.                                                                          | Depends on **technology**, **load**, and **input slew**.      |
| **Clock Skew (T<sub>skew</sub>)**                   | Difference in **clock arrival times** between launch and capture flip-flops.            | Positive skew helps setup but hurts hold; negative skew does opposite. | Added/subtracted in both equations as shown.                                                                    | Controlled by **Clock Tree Synthesis (CTS)**.                 |
| **Clock Jitter / Uncertainty (T<sub>jitter</sub>)** | Variation in the **actual clock edge timing** from its ideal position.                  | Reduces setup margin and tightens hold checks.                         | Appears as negative in setup, positive in hold equation.                                                        | Modeled using STA constraint: `set_clock_uncertainty <value>` |


---

###  On-Chip Variation (OCV) in STA

On-Chip Variation (OCV) refers to the **timing differences** among identical cells or paths within the same chip caused by **manufacturing process variations** such as **etching** and **oxide thickness** variation.


####  Relation:
t_PD = f(R) = f(I_d) = f(t_ox, W, L)



#### Chain of Effect:
1. **Variation Sources:**
   - **Etching variation** → changes **channel length (L)**
   - **Oxide thickness variation (tₒₓ)** → changes **gate capacitance (Cₒₓ)**

2. **These variations cause change in drain current:**
   I_d = μ * (ε_ox / t_ox) * (W / L) * [(V_gs - V_t)V_ds - (V_ds² / 2)]

3. **Change in drain current (I_d) affects resistance:**
   R ∝ 1 / I_d

4. **Change in resistance affects delay:**
   t_pd = f(R)



####  Final Impact:
→ Variation in t_ox, W, L → causes change in I_d  
→ which changes R  
→ and finally affects **propagation delay (t_pd)**



#### Example:
| Corner Type | Delay | Variation |
|--------------|--------|------------|
| Fast Corner | 91 ps | -9% |
| Typical | 100 ps | 0% |
| Slow Corner | 108 ps | +8% |

**OCV derates = +8%, -9%**


**On-Chip Variation (OCV)** arises due to **etching** and **oxide thickness** variations in fabrication.  
These cause transistor dimension changes (t_ox, W, L), leading to **drain current variation**, which alters **resistance** and thus **propagation delay (t_pd)**.  
In STA, OCV is modeled using **derating factors** (e.g., +8%, -9%) to account for these variations in setup and hold time analysis.

---


### Setup Analysis with Clock Pull-Up / Push-Out (OCV)

#### Concept:
In **Setup Timing Analysis**, data must arrive **before** the next active clock edge at the capture flip-flop.  
OCV (On-Chip Variation) causes clock paths to vary in delay, leading to different timing cases.



#### Two OCV Clock Scenarios in Setup Analysis:

##### Clock Pull-Up (Capture Clock Early)
- Capture clock arrives **earlier** due to OCV.
- Launch clock remains typical or slower.
- The **available setup time window reduces**, increasing the chance of **setup violation**.
- Used for **worst-case setup** analysis.

**Effect:**  
Smaller time between launch and capture → Setup margin ↓



##### Clock Push-Out (Capture Clock Late)
- Capture clock arrives **later** due to OCV.
- Launch clock remains typical or faster.
- The **available setup time increases**, so **setup timing improves**.
- Used for **best-case setup** analysis.

**Effect:**  
Larger time between launch and capture → Setup margin ↑



#### Setup Timing Equation:
\[
(\theta + Δ₁) < (T + Δ₂) - S - SU
\]
Where:
- \( Δ₁ \): Data path delay  
- \( Δ₂ \): Clock path delay  
- \( S \): Skew  
- \( SU \): Setup time of flip-flop  

**Slack = (Data Required Time) – (Data Arrival Time)**  
Slack ≥ 0 → No violation ✅  
Slack < 0 → Setup violation ❌  


### Pessimism Removal (CPPR – Common Path Pessimism Removal)

#### Concept:
- In STA, OCV derates are applied **independently** to launch and capture clock paths.  
- However, **a part of the clock path is common** to both flops.  
- The tool may assign **different OCV delays** to this **common path**, which is **physically impossible**.


####  Example:
If common clock path delay is 100 ps:
- Launch clock path derated to **+8%** → 108 ps  
- Capture clock path derated to **–9%** → 91 ps  

→ Same common segment cannot be both 108 ps and 91 ps simultaneously.  
This adds **extra pessimism** in analysis.


#### Solution — CPPR:
- Compute the difference in the **common clock path delay** (108 – 91 = 17 ps).  
- **Subtract this difference** from the total setup path delay.  

This **removes the pessimism** and makes timing more realistic.


### Summary:

- **Clock Pull-Up:** Capture clock early → Setup margin decreases (worst case).  
- **Clock Push-Out:** Capture clock late → Setup margin increases (best case).
- **CPPR:** Removes false pessimism due to different OCV values on common clock segments by adjusting (adding/subtracting) the difference.



---

### Hold Analysis with Clock Pull-Up / Push-Down (OCV)

#### Concept:
In **Hold Timing Analysis**, data launched by a clock edge must **not reach the capture flip-flop too soon** after the same edge.  
OCV (On-Chip Variation) can alter clock arrival times, creating worst-case or best-case hold situations.



#### Two OCV Clock Scenarios in Hold Analysis:

##### Clock Pull-Up (Launch Clock Late)
- **Launch clock** is delayed (slower path due to OCV).  
- Capture clock remains typical.  
- Data launches late → capture happens before the data stabilizes.  
- **Hold margin decreases** → Possible **hold violation**.

**Effect:**  
Launch delay ↑ → Data arrives later → Hold margin ↓


##### Clock Push-Down (Capture Clock Early)
- **Capture clock** arrives earlier (faster path due to OCV).  
- Launch clock remains typical.  
- Capture occurs too early → Data not stable yet.  
- **Hold margin decreases** further.

**Effect:**  
Capture early → Sampling window shortens → Hold margin ↓



### Worst-Case Hold Scenario:
- **Launch Clock → Pull-Up (late)**  
- **Capture Clock → Push-Down (early)**  

This combination minimizes hold slack and is used in STA for **worst-case hold timing**.


### Hold Timing Equation:
\[
(\theta + Δ₁) > (\theta + Δ₂) + H
\]
Where:
- \( Δ₁ \): Data path delay  
- \( Δ₂ \): Clock path delay  
- \( H \): Hold time of the flip-flop  

**Slack = (Data Required Time) – (Data Arrival Time)**  
Slack ≥ 0 → No violation ✅  
Slack < 0 → Hold violation ❌  


###  Pessimism Removal (CPPR – Common Path Pessimism Removal)

#### Concept:
In hold analysis, OCV derates are also applied separately to **launch** and **capture** clock paths.  
If both clocks share a **common portion** of the clock tree, this portion might incorrectly get **different OCV delays**, causing **extra pessimism**.


#### Example:
If the common clock path delay = 80 ps:  
- Launch clock derated by +8% → 86.4 ps  
- Capture clock derated by –9% → 72.8 ps  

The difference (86.4 – 72.8 = 13.6 ps) is **artificial**, since both paths share the same clock segment.


#### Solution — CPPR:
- The **difference in common path delay (13.6 ps)** is **added back** to the hold slack.  
- This adjustment **removes pessimism** and gives a realistic hold result.



#### Summary:
- **Clock Pull-Up:** Launch clock late → Hold margin ↓  
- **Clock Push-Down:** Capture clock early → Hold margin ↓  
- **Worst Case:** Pull-Up (launch late) + Push-Down (capture early)  
- **CPPR:** Eliminates false pessimism by adjusting for the common clock path having different derates.


---
