# Static Timing Analysis (STA)

Static Timing Analysis (STA) is a method used to verify the timing performance of a digital circuit without applying input vectors. It checks whether all data paths meet the required timing constraints for correct operation at a given clock frequency. STA analyzes every possible timing path between start points (like flip-flop clock pins or input ports) and end points (like flip-flop data pins or output ports), ensuring that signals propagate within the allowed time limits.

Static Timing Analysis (STA) is a process used to verify that a digital circuit meets its timing requirements under all conditions, without running simulations. STA involves three main aspects — **timing checks, constraints, and library delay models**.

Timing checks ensure signals arrive at the correct time (like setup and hold checks).

Constraints define the required timing conditions, such as clock period, input/output delays, and margins.

Library delay models (like NLDM and Current Source models) describe how cell delays vary with load and input transition, enabling accurate timing prediction.
