# CMOS Inverter Robustness – (iii) Power Supply Scaling


Based on the provided images and SPICE netlist for the CMOS inverter robustness evaluation with power supply scaling, here are the notes for the case where the power supply voltage ($V_{DD}$) is **$0.5\text{V}$**.

## CMOS Inverter Robustness: Power Supply Scaling to $0.5\text{V}$

The SPICE simulation explores the effect of scaling down the power supply voltage ($V_{DD}$) on a CMOS inverter, specifically highlighting the advantages and disadvantages when $V_{DD}$ is reduced to $0.5\text{V}$. The DC characteristics plot shows multiple transfer curves corresponding to $V_{DD}$ values from $2\text{V}$ down to $0.5\text{V}$ (labeled as **dc1 out** through **dc4 out**). The curve corresponding to the lowest $V_{DD}$ is the one of interest.

### 1. Advantages of Using $0.5\text{V}$ Supply

The most significant benefit of power supply scaling is the reduction in **dynamic power consumption** and an unexpected **increase in voltage gain**.

* **Significant Reduction in Energy/Power:**
    * **Energy Formula:** The dynamic energy consumed per switching event is approximated by the formula: $E = \frac{1}{2} C V_{DD}^2$, where $C$ is the load capacitance and $V_{DD}$ is the supply voltage.
    * **Energy Reduction:** Scaling $V_{DD}$ from a baseline (e.g., $2\text{V}$ or a higher voltage not explicitly shown, but implied by the context of scaling) to $0.5\text{V}$ results in a quadratic reduction in dynamic energy.
    * **Power Reduction:** Dynamic power, $P_{dynamic} = E \cdot f \cdot \alpha$ (where $f$ is frequency and $\alpha$ is activity factor), also decreases quadratically with $V_{DD}$.

* **Increase in Voltage Gain:**
    * The voltage gain ($A_v$) is the slope of the DC transfer characteristic in the transition region.
    * **Measured Gain:** The simulation results show that for $V_{DD} = 0.5\text{V}$, the gain is **$5.81$**.
    * **Improvement:** A substantial improvement in gain, showing a $50\%$ improvement from Vdd = 2V.
    * **Reason:** At low $V_{DD}$, the drain-source voltage ($V_{DS}$) is small, pushing the MOSFETs deeper into the **saturation region** (for a brief range) where the transconductance ($g_m$) is high, leading to a larger gain: $A_v \approx -g_{m}(R_{out})$.

---

### 2. Disadvantages and Performance Impact

While energy is significantly reduced, the major drawback of scaling $V_{DD}$ down to $0.5\text{V}$ is the **impact on circuit performance**, primarily in terms of speed.

* **Performance Impact (Increased Delay):**
    * The **rise delay** and **fall delay** of the inverter, which together determine the circuit's operating speed, are negatively affected.
    * **Reason:** The charge/discharge current ($I$) available to charge the load capacitance ($C_{load}$) is reduced with lower $V_{DD}$, as $I \propto (V_{DD} - V_{th})^2$ (where $V_{th}$ is the threshold voltage).
    * Since delay ($\tau$) is proportional to $C_{load} V_{DD} / I$, a reduction in $V_{DD}$ and the corresponding reduction in current $I$ (which is a strong function of $V_{DD}$) leads to a significant **increase in propagation delay**.
    * The transient response plot (image\_a859fa.png) visually confirms the presence of **rise delay** and **fall delay**—the circuit is slower to switch states. This is noted as the **"Prime topic of discussion"** among the disadvantages.

* **Increased Sensitivity to Noise and Process Variation:**
    * As the difference between $V_{DD}$ and the transistor threshold voltage ($V_{th}$) shrinks, the circuit becomes more susceptible to **noise** (reducing the noise margin) and **process variations** (making the performance less predictable across manufactured chips).

## Spice simulation
### Netlist 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.495um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 0.1fF

Vdd vdd 0 2V
Vin in 0 2V

.control 
let powerSupply = 2 
alter Vdd = powerSupply

    let voltageSupplyVariation = 0
    dowhile voltageSupplyVariation < 4
        dc Vin 0 2 0.01
        let powerSupply = powerSupply - 0.5
        alter Vdd = powerSupply
        let voltageSupplyVariation = voltageSupplyVariation + 1 
    end
    
    plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in  xlabel "Input Voltage[V]" ylabel "Output Voltage[V]" title "Inverter DC Characteristics as a function of supply voltage"
    * quit 
.endc 
.end
```

<img width="746" height="684" alt="image" src="https://github.com/user-attachments/assets/044f7960-1374-46a8-b70d-0fb6c67a96d7" />

for vdd = 2v , gain = 2.9
for vdd = 1.5v , gain = 3.6
for vdd = 1v , gain = 5.67
for vdd = 0.5v , gain = 5.81
