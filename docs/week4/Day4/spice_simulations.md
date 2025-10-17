

---

# Case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

## Plot : VTC Curve 

<img width="551" height="620" alt="image" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />

From graph => Vm = 0.7V, 
Voh = 1.8V , Vol = 0V , Vil = 0.375V , Vih =  0.820V


---

# Case 2 : Wn=0.195um,Wp=0.39um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 2 ( $\frac{Wp}{Lp}$ )



## Plot : VTC Curve 

<img width="585" height="640" alt="image" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

From graph , Vm = 0.86V , 
Voh = 1.8V , Vol = 0V , Vil = 0.632V , Vih =  1.018V

---

# Case 3 : Wn=0.195um,Wp=0.585um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 3 ( $\frac{Wp}{Lp}$ )

## Plot : VTC Curve 

<img width="627" height="684" alt="image" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />


From graph , Vm = 0.95V ,
Voh = 1.8V , Vol = 0V , Vil = 0.728V , Vih =  1.148V

---

# Case 4 : Wn=0.195um,Wp=0.78um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 4 ( $\frac{Wp}{Lp}$ )

## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />


From graph , Vm = 1.01V ,
Voh = 1.8V , Vol = 0V , Vil = 0.851V , Vih =  1.20V

---

# Case 5 : Wn=0.195um,Wp=0.975um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 5 ( $\frac{Wp}{Lp}$ )


## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

From graph , Vm = 1.06V ,
Voh = 1.8V , Vol = 0V , Vil = 0.897V , Vih =  1.31V

---


# 📘 CMOS Inverter – Static Behaviour Evaluation

This report presents the **Voltage Transfer Characteristics (VTC)** of a CMOS inverter designed and simulated using **130nm technology**.  
All simulations were performed in **NGSPICE** using the SkyWater 130nm model file.

---

##  Objective

To evaluate the **switching threshold (Vm)** and **noise margins (NMH, NML)** of a CMOS inverter by varying the ratio of $\frac{W_p}{W_n}$.


##  Theory

At the **switching threshold** $V_M$, both NMOS and PMOS transistors operate in saturation.  
The drain currents of NMOS and PMOS are equal in magnitude:

$$
\mathbf{
k_p \left[ (V_M - V_{DD} - V_t) V_{dsatp} - \frac{V_{dsatp}^2}{2} \right]
+ k_n \left[ (V_M - V_t) V_{dsatn} - \frac{V_{dsatn}^2}{2} \right]
= 0
}
$$

---

##  Simulation Parameters

| Parameter | Value |
|------------|--------|
| Technology | 130nm CMOS |
| $V_{DD}$ | 1.8V |
| $L_n = L_p$ | 65nm |
| Tool Used | NGSPICE |
| Temperature | 27°C |
| Input Sweep | 0 → 1.8V |

---

##  Case Studies

### **Case 1:**  
$W_n = W_p = 0.195\mu m$, $L_n = L_p = 65nm$  
$\Rightarrow \frac{W_n}{L_n} = \frac{W_p}{L_p}$  

**VTC Curve:**
<img width="551" height="620" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />

**Results:**
- $V_M = 0.70V$  
- $V_{OH} = 1.8V$ , $V_{OL} = 0V$  
- $V_{IL} = 0.375V$ , $V_{IH} = 0.820V$

**Noise Margins:**
- $NM_H = V_{OH} - V_{IH} = 1.8 - 0.82 = 0.98V$
- $NM_L = V_{IL} - V_{OL} = 0.375 - 0 = 0.375V$

---

### **Case 2:**  
$W_n=0.195\mu m$, $W_p=0.39\mu m$, $L_n=L_p=65nm$  
$\Rightarrow \frac{W_n}{L_n} = 2 \times \frac{W_p}{L_p}$

**VTC Curve:**
<img width="585" height="640" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

**Results:**
- $V_M = 0.86V$
- $V_{IL} = 0.632V$, $V_{IH} = 1.018V$
- $V_{OH} = 1.8V$, $V_{OL} = 0V$

**Noise Margins:**
- $NM_H = 1.8 - 1.018 = 0.782V$
- $NM_L = 0.632 - 0 = 0.632V$

---

### **Case 3:**  
$W_n=0.195\mu m$, $W_p=0.585\mu m$, $L_n=L_p=65nm$  
$\Rightarrow \frac{W_n}{L_n} = 3 \times \frac{W_p}{L_p}$

**VTC Curve:**
<img width="627" height="684" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />

**Results:**
- $V_M = 0.95V$
- $V_{IL} = 0.728V$, $V_{IH} = 1.148V$
- $V_{OH} = 1.8V$, $V_{OL} = 0V$

**Noise Margins:**
- $NM_H = 1.8 - 1.148 = 0.652V$
- $NM_L = 0.728 - 0 = 0.728V$

---

### **Case 4:**  
$W_n=0.195\mu m$, $W_p=0.78\mu m$, $L_n=L_p=65nm$  
$\Rightarrow \frac{W_n}{L_n} = 4 \times \frac{W_p}{L_p}$

**VTC Curve:**
<img width="632" height="684" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />

**Results:**
- $V_M = 1.01V$
- $V_{IL} = 0.851V$, $V_{IH} = 1.20V$
- $V_{OH} = 1.8V$, $V_{OL} = 0V$

**Noise Margins:**
- $NM_H = 1.8 - 1.20 = 0.60V$
- $NM_L = 0.851 - 0 = 0.851V$

---

### **Case 5:**  
$W_n=0.195\mu m$, $W_p=0.975\mu m$, $L_n=L_p=65nm$  
$\Rightarrow \frac{W_n}{L_n} = 5 \times \frac{W_p}{L_p}$

**VTC Curve:**
<img width="632" height="684" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

**Results:**
- $V_M = 1.06V$
- $V_{IL} = 0.897V$, $V_{IH} = 1.31V$
- $V_{OH} = 1.8V$, $V_{OL} = 0V$

**Noise Margins:**
- $NM_H = 1.8 - 1.31 = 0.49V$
- $NM_L = 0.897 - 0 = 0.897V$

---

##  Summary Table

| Case | $\frac{W_p}{W_n}$ | $V_M$ (V) | $V_{IL}$ (V) | $V_{IH}$ (V) | $NM_H$ (V) | $NM_L$ (V) |
|------|--------------------|------------|---------------|---------------|-------------|-------------|
| 1 | 1 | 0.70 | 0.375 | 0.820 | 0.98 | 0.375 |
| 2 | 0.5 | 0.86 | 0.632 | 1.018 | 0.782 | 0.632 |
| 3 | 0.33 | 0.95 | 0.728 | 1.148 | 0.652 | 0.728 |
| 4 | 0.25 | 1.01 | 0.851 | 1.20 | 0.60 | 0.851 |
| 5 | 0.20 | 1.06 | 0.897 | 1.31 | 0.49 | 0.897 |

---

##  Observations

- As $\frac{W_p}{W_n}$ **decreases**, PMOS becomes **weaker**, shifting $V_M$ **toward VDD**.  
- **Noise margin low (NML)** **increases**, whereas **noise margin high (NMH)** **decreases**.  
- Optimum noise immunity is achieved when $\frac{W_p}{W_n}$ ratio is such that $V_M \approx \frac{V_{DD}}{2}$.


- The **switching threshold** of a CMOS inverter depends on the $\frac{W_p}{W_n}$ ratio.  
- For **balanced transfer characteristics**, $V_M$ should be around $V_{DD}/2$.  
- The study confirms that **increasing PMOS width** (higher $\frac{W_p}{W_n}$) improves **NMH** but reduces **NML**.

