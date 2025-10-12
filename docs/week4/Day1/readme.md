# Circuit design and spice simulation 

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

## Basic Circuit Element - NMOS

### 1. NMOS: Basic Structure and Operation

The **N-channel Metal-Oxide-Semiconductor (NMOS)** transistor is a basic circuit element, essentially a voltage-controlled switch.

* **Structure:** It consists of two heavily doped n-type regions (**Source 'S'** and **Drain 'D'**) embedded in a p-type semiconductor substrate (**Bulk 'B'**). A metal or polysilicon **Gate 'G'** is separated from the substrate by a thin layer of silicon dioxide (oxide).
* **Function:** It controls the flow of electrons (current) between the Source and Drain using the voltage applied to the Gate.
* **Default State ($V_{GS}=0$):** With the Gate, Source, Drain, and Bulk connected to ground, the p-substrate/n-regions form a reversed-biased **p-n junction**. The resistance between Source and Drain is very **high** (switch is **OFF**).



### 2. Strong Inversion and Threshold Voltage ($V_t$)

To turn the NMOS switch **ON**, a positive voltage ($V_{GS}$) is applied to the Gate.

* **Depletion:** The positive Gate voltage attracts negative charge carriers (electrons) and repels positive charge carriers (holes) away from the surface of the p-substrate, forming a **depletion region** beneath the Gate.
* **Strong Inversion:** As $V_{GS}$ increases, the surface beneath the Gate becomes so strongly electron-rich that its electrical properties effectively **invert** from p-type to n-type material. This forms a continuous n-channel connecting the Source and Drain.
* **Threshold Voltage ($V_t$):** The specific **Gate-to-Source voltage ($V_{GS}$)** at which **strong inversion** occurs and the channel starts to conduct is called the **Threshold Voltage ($V_t$)**.
    * For $V_{GS} < V_t$, the transistor is **OFF**.
    * For $V_{GS} \ge V_t$, the transistor is **ON**, and its conductivity is modulated by $V_{GS}$.



### 3. Threshold Voltage with Substrate (Body) Effect

The Threshold Voltage ($V_t$) is not a fixed value; it changes if there is a voltage difference between the Source and the Bulk (Substrate). This phenomenon is called the **Body Effect** or **Substrate Effect**.

* **Condition:** The effect occurs when the **Source-to-Bulk voltage ($V_{SB}$) is a positive value** (i.e., the Source is at a higher potential than the Bulk).
* **Effect on Depletion:** A positive $V_{SB}$ creates an **additional reverse bias** between the Source and Bulk. This increases the width of the depletion region near the Source.
* **Effect on $V_t$:** To achieve the same level of strong inversion and form a channel, a **higher Gate voltage ($V_{GS}$)** is now required to counteract the larger depletion region.
    $$\text{A positive } V_{SB} \Rightarrow \text{Higher } V_t \Rightarrow \text{Harder to turn the transistor ON}$$

The **Threshold Voltage Equation** expresses this relationship:

$$V_t = V_{t0} + \gamma (\sqrt{|-2\Phi_f + V_{sb}|} - \sqrt{|-2\Phi_f|})$$

| Term | Description |
| :--- | :--- |
| $\mathbf{V_t}$ | New Threshold Voltage. |
| $\mathbf{V_{t0}}$ | Threshold Voltage when $V_{sb}=0$. |
| $\mathbf{\gamma}$ | **Body Effect Coefficient** (Expresses the impact of $V_{sb}$ changes). |
| $\mathbf{V_{sb}}$ | Source-to-Bulk/Substrate voltage (the cause of the Body Effect). |
| $\mathbf{\Phi_f}$ | Fermi Potential (a semiconductor physics parameter). |
