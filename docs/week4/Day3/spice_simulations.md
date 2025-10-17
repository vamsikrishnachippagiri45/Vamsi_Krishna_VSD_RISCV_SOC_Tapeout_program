# CMOS Inverter Robustness – (i)Switching Threshold (Vm) and Delays(rise,fall)


This experiment evaluates the **static behavior** of a CMOS inverter using 130nm CMOS technology.  
The analysis includes:
- **Voltage Transfer Characteristics (VTC)**
- **Transient Analysis**
- **Observation of Switching Threshold (Vm)**
- **Rise and Fall Time behavior** for different sizing ratios.



## Simulation Setup

- **Technology:** 130nm CMOS  
- **Supply Voltage (VDD):** 1.8V  
- **Load Capacitance (Cload):** 50fF  
- **Input:** DC sweep for VTC, PULSE input for transient  
- **Model File:** `cmos_130nm.txt` (included in Day3 folder)

---

# Case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.195um l=0.13um
M2 out in 0 0 nmos w=0.195um l=0.13um
Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc
display
.endc

.end
```
<img width="942" height="802" alt="image" src="https://github.com/user-attachments/assets/b904853b-075d-434e-b880-5d0774b7799e" />

## Plot : VTC Curve 

<img width="551" height="620" alt="image" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />


From graph , Vm = 0.7V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.195um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

<img width="942" height="659" alt="image" src="https://github.com/user-attachments/assets/28fa63e7-e240-4a8f-a8e7-0c9c5822e95f" />

## plot : transient analysis

<img width="821" height="574" alt="image" src="https://github.com/user-attachments/assets/cb90748b-989d-425b-94a9-c950f39d3c13" />

Rise time = 0.368ns

Fall time = 0.180ns

| Parameter | Value |
|------------|--------|
| Vm | 0.7V |
| Rise Time | 0.368ns |
| Fall Time | 0.180ns |

**Observation:**
- The inverter is **symmetric** in design (equal drive strengths).
- Switching threshold (~0.7V) is **less than VDD/2** due to higher mobility of NMOS.
- Rise time is **higher than fall time** because PMOS is weaker.


---

# Case 2 : Wn=0.195um,Wp=0.39um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 2 ( $\frac{Wn}{Ln}$ )

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.39um l=0.13um
M2 out in 0 0 nmos w=0.195um l=0.13um
Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc
display
.endc

.end
```

## Plot : VTC Curve 

<img width="585" height="640" alt="image" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

From graph , Vm = 0.86V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.39um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

## plot : transient analysis

<img width="835" height="640" alt="image" src="https://github.com/user-attachments/assets/74b89a9b-2597-4313-9578-cc0284482600" />


Rise time = 0.194ns

Fall time = 0.173ns


| Parameter | Value |
|------------|--------|
| Vm | 0.86V |
| Rise Time | 0.194ns |
| Fall Time | 0.173ns |

**Observation:**
- Increasing PMOS width increases its drive strength.
- The switching threshold **moves closer to mid-supply**.
- Rise time decreases as PMOS becomes stronger, resulting in **more balanced transition times**.


---

# Case 3 : Wn=0.195um,Wp=0.585um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 3 ( $\frac{Wn}{Ln}$ )

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.585um l=0.13um
M2 out in 0 0 nmos w=0.195um l=0.13um
Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc
display
.endc

.end
```

## Plot : VTC Curve 

<img width="627" height="684" alt="image" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />


From graph , Vm = 0.95V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.585um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

## plot : transient analysis

<img width="872" height="659" alt="image" src="https://github.com/user-attachments/assets/3ff50515-31ae-488b-aa53-235e7c92d638" />

Rise time = 0.125ns

Fall time = 0.187ns



| Parameter | Value |
|------------|--------|
| Vm | 0.95V |
| Rise Time | 0.125ns |
| Fall Time | 0.187ns |

**Observation:**
- Further increase in PMOS width shifts Vm **towards VDD/2** and slightly above.
- Rise time improves due to stronger pull-up action.
- Fall time remains almost constant since NMOS size is unchanged.

---

# Case 4 : Wn=0.195um,Wp=0.78um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 4 ( $\frac{Wn}{Ln}$ )

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.78um l=0.13um
M2 out in 0 0 nmos w=0.195um l=0.13um
Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc
display
.endc

.end
```

## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />


From graph , Vm = 1.01V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.78um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

## plot : transient analysis

<img width="833" height="612" alt="image" src="https://github.com/user-attachments/assets/98d89694-4fb2-429e-9128-21887e41d454" />

Rise time = 0.097ns

Fall time = 0.194ns


| Parameter | Value |
|------------|--------|
| Vm | 1.01V |
| Rise Time | 0.097ns |
| Fall Time | 0.194ns |

**Observation:**
- The inverter becomes **PMOS-dominated**, so switching threshold moves **above mid-supply**.
- Rise time reduces significantly due to stronger PMOS drive.
- Fall time slightly increases since NMOS current is relatively weaker.

---

# Case 5 : Wn=0.195um,Wp=0.975um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 5 ( $\frac{Wn}{Ln}$ )

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.975um l=0.13um
M2 out in 0 0 nmos w=0.195um l=0.13um
Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc
display
.endc

.end
```

## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

From graph , Vm = 1.06V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.975um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

## plot : transient analysis

<img width="835" height="635" alt="image" src="https://github.com/user-attachments/assets/9f8d8827-cfe0-43e6-9b5d-2f1fc8bb6ef9" />

Rise time = 0.090ns

Fall time = 0.188ns

| Parameter | Value |
|------------|--------|
| Vm | 1.06V |
| Rise Time | 0.090ns |
| Fall Time | 0.188ns |

**Observation:**
- With large PMOS width, **switching point shifts further high**.
- Rise time becomes very small — PMOS quickly pulls the output high.
- Fall time remains almost same since NMOS size is constant.
- The inverter becomes **unbalanced** (PMOS dominates).

---

##  Observations

| Case | (Wp/Wn) Ratio | Vm (V) | Rise Time (ns) | Fall Time (ns) |
|------|---------------|--------|----------------|----------------|
| 1 | 1 | 0.70 | 0.368 | 0.180 |
| 2 | 2 | 0.86 | 0.194 | 0.173 |
| 3 | 3 | 0.95 | 0.125 | 0.187 |
| 4 | 4 | 1.01 | 0.097 | 0.194 |
| 5 | 5 | 1.06 | 0.090 | 0.188 |

**Trend Summary:**
- As **Wp/Wn increases**, the **switching threshold Vm** moves **closer to VDD/2 and above**.  
- **Rise time decreases** because the stronger PMOS improves the pull-up speed.  
- **Fall time remains nearly constant**, limited by NMOS size.  
- Optimum inverter symmetry is achieved when **Wp/Wn ≈ 2–3**, giving Vm ≈ 0.9V.

---

##  Conclusion

Balancing **rise and fall times** requires matching the **effective drive strengths** of PMOS and NMOS.  
Since electron mobility (µn) ≈ 2–3× hole mobility (µp), PMOS width is typically chosen **2–3× wider** than NMOS.
  
For a 130nm CMOS inverter, choosing **Wp ≈ 2–3 × Wn** gives:
- Switching threshold ≈ 0.9V (≈ VDD/2)
- Nearly equal rise and fall times  
→ Hence, the inverter operates **symmetrically and efficiently**.

---

