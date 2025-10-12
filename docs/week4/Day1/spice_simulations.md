# Spice simulation 

---

## Introduction to SPICE Simulation

SPICE (Simulation Program with Integrated Circuit Emphasis) is an industry-standard circuit simulator developed at UC Berkeley for analog and mixed-signal analysis. It solves circuit equations (using nodal analysis and Newton-Raphson methods) from a netlist to find voltages and currents at each node.

Types of Analyses:
DC Analysis: Finds steady-state operating point , 
AC Analysis: Analyzes frequency response (filters, amplifiers) , 
Transient Analysis: Studies time-domain behavior (waveforms, rise/fall times).

Why SPICE Simulations Are Needed in IC Design : 

1) To Understand Cell Delay:
Delay comes from internal transistor switching and output load capacitance (fanout + interconnects).
SPICE uses accurate transistor models to calculate true propagation delay under specific conditions (input slew & output load).

2) To Ensure Delay Models Are Accurate:
STA tools use pre-characterized delay tables (.lib) generated using SPICE simulations.
SPICE is the “gold standard” for building these models.

3) To Verify STA Results (Signoff):
SPICE verifies critical timing paths detected by STA.
Final signoff simulations (FastSPICE/full SPICE) ensure accurate timing before tapeout.

---
