
# Noise Margin in Digital Logic Circuits

##  Concept of Noise Margin

**Noise Margin** is a measure of a digital circuit's **immunity to noise** — unwanted voltage fluctuations due to crosstalk, electromagnetic interference (EMI), power supply variation, or signal reflection.

It quantifies how much **noise voltage** can be tolerated before logic levels are misinterpreted.   
**Higher noise margin = more reliable and robust circuit.**

---

## Key Voltage Parameters

Digital gates are defined by four key voltage levels:

### **1. Output Voltage Levels**
- **\(V_{OH}\)** — *Voltage Output High:*  
  Minimum output voltage guaranteed as logic **‘1’**.  
- **\(V_{OL}\)** — *Voltage Output Low:*  
  Maximum output voltage guaranteed as logic **‘0’**.  

### **2. Input Voltage Levels**
- **\(V_{IH}\)** — *Voltage Input High:*  
  Minimum input voltage reliably interpreted as logic **‘1’**.  
- **\(V_{IL}\)** — *Voltage Input Low:*  
  Maximum input voltage reliably interpreted as logic **‘0’**.  

---

##  Defining the Regions and Margins

The voltage axis is divided into **three regions**,  in the inverter’s **Voltage Transfer Characteristic (VTC)** curve.

### **1. Undefined (Indeterminate) Region**
Region between \(V_{IL}\) and \(V_{IH}\):  
If the signal lies here, the output becomes **unpredictable** — may oscillate or produce incorrect logic.


### **2. Noise Margins**

Noise margins define how much noise can be tolerated before logic recognition fails.

#### **Noise Margin Low (NML):**
Protection for the **logic ‘0’** state.

$$
NM_L = V_{IL} - V_{OL}
$$

Represents the maximum noise voltage that can be added to \(V_{OL}\) (output low)  
without exceeding \(V_{IL}\) (input low threshold).


#### **Noise Margin High (NMH):**
Protection for the **logic ‘1’** state.

$$
NM_H = V_{OH} - V_{IH}
$$

Represents the maximum noise voltage that can be subtracted from \(V_{OH}\) (output high)  
without falling below \(V_{IH}\) (input high threshold).

---

##  Transfer Curve and Slope Condition

The **Voltage Transfer Characteristic (VTC)** plots $\(V_{OUT}\)$ vs. $\(V_{IN}\)$.

- The points $\(V_{IL}\)$ and $\(V_{IH}\)$ are defined where the **slope = –1**, i.e.:
  
$$
\left| \frac{dV_{OUT}}{dV_{IN}} \right| = 1
$$

- These points mark the boundaries between low-gain and high-gain regions.

Defining $\(V_{IL}\)$ and $\(V_{IH}\)$ at slope = –1 ensures **maximum noise immunity** and **optimal switching performance**.

---

##  Noise Immunity Illustration

### **Case (a): Logic '0' — Correct Recognition**
Noise bump < $\(NM_L\)$.  
Signal remains below $\(V_{IL}\)$ → Still interpreted as logic ‘0’.

### **Case (b): Undefined Logic**
Noise bump > $\(NM_L\)$, entering region between $\(V_{IL}\)$ and $\(V_{IH}\)$.  
Signal becomes **unpredictable**, may toggle between ‘0’ and ‘1’.

### **Case (c): Logic '1' — Correct Recognition**
Noise < $\(NM_H\)$.  
Signal remains above $\(V_{IH}\)$ → Correctly interpreted as logic ‘1’.

---

##  Summary

| Parameter | Definition | Meaning |
|------------|-------------|----------|
| $\(V_{OH}\)$ | Min output voltage for logic ‘1’ | Output HIGH level |
| $\(V_{OL}\)$ | Max output voltage for logic ‘0’ | Output LOW level |
| $\(V_{IH}\)$ | Min input voltage recognized as logic ‘1’ | Input HIGH threshold |
| $\(V_{IL}\)$ | Max input voltage recognized as logic ‘0’ | Input LOW threshold |
| $\(NM_H\)$ | $\(V_{OH} - V_{IH}\)$ | Noise margin for logic ‘1’ |
| $\(NM_L\)$ | $\(V_{IL} - V_{OL}\)$ | Noise margin for logic ‘0’ |

**Larger noise margins** ⇒ **better noise immunity** and **robust digital operation.**
