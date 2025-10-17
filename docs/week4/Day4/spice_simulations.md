

---

# Case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wp}{Lp}$ = $\frac{Wn}{Ln}$ 

## Plot : VTC Curve 

<img width="551" height="620" alt="image" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />

From graph => Vm = 0.7V, 
Voh = 1.8V , Vol = 0V , Vil = 0.375V , Vih =  0.820V


---

# Case 2 : Wn=0.195um,Wp=0.39um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 2 ( $\frac{Wn}{Ln}$ )



## Plot : VTC Curve 

<img width="585" height="640" alt="image" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

From graph , Vm = 0.86V , 
Voh = 1.8V , Vol = 0V , Vil = 0.632V , Vih =  1.018V

---

# Case 3 : Wn=0.195um,Wp=0.585um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 3 ( $\frac{Wn}{Ln}$ )

## Plot : VTC Curve 

<img width="627" height="684" alt="image" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />


From graph , Vm = 0.95V ,
Voh = 1.8V , Vol = 0V , Vil = 0.728V , Vih =  1.148V

---

# Case 4 : Wn=0.195um,Wp=0.78um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 4 ( $\frac{Wn}{Ln}$ )

## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />


From graph , Vm = 1.01V ,
Voh = 1.8V , Vol = 0V , Vil = 0.851V , Vih =  1.20V

---

# Case 5 : Wn=0.195um,Wp=0.975um,Ln=Lp=65nm, ( $\frac{Wp}{Lp}$ ) = 5 ( $\frac{Wn}{Ln}$ )


## Plot : VTC Curve 

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

From graph , Vm = 1.06V ,
Voh = 1.8V , Vol = 0V , Vil = 0.897V , Vih =  1.31V

---

# 🧩 CMOS Inverter — Static Behaviour Evaluation (Corrected Wp / Wn)

> **Note:** This document uses the corrected device sizing:
> - Wn = 0.195 µm (constant)  
> - Wp = 0.195, 0.39, 0.585, 0.78, 0.975 µm  
> - Therefore, (Wp/Wn) = 1, 2, 3, 4, 5

---

## ⚙️ Simulation Parameters

- Technology: **130 nm**
- VDD = **1.8 V**
- Ln = Lp = **65 nm**
- Tool: **NGSPICE (Sky130 models)**
- Input Sweep: 0 → 1.8 V

---

## 📈 Case-by-Case VTC Results

### **Case 1 — (Wp/Wn = 1)**  
Wn = 0.195 µm, Wp = 0.195 µm

**VTC Plot:**  
<img width="551" height="620" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />

**From Graph:**  
- Vm = 0.70 V  
- Voh = 1.8 V, Vol = 0 V  
- Vil = 0.375 V, Vih = 0.820 V  

**Noise Margins:**  
- NMH = Voh − Vih = **0.98 V**  
- NML = Vil − Vol = **0.375 V**

---

### **Case 2 — (Wp/Wn = 2)**  
Wn = 0.195 µm, Wp = 0.39 µm

**VTC Plot:**  
<img width="585" height="640" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

**From Graph:**  
- Vm = 0.86 V  
- Voh = 1.8 V, Vol = 0 V  
- Vil = 0.632 V, Vih = 1.018 V  

**Noise Margins:**  
- NMH = 1.8 − 1.018 = **0.782 V**  
- NML = 0.632 − 0 = **0.632 V**

---

### **Case 3 — (Wp/Wn = 3)**  
Wn = 0.195 µm, Wp = 0.585 µm

**VTC Plot:**  
<img width="627" height="684" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />

**From Graph:**  
- Vm = 0.95 V  
- Voh = 1.8 V, Vol = 0 V  
- Vil = 0.728 V, Vih = 1.148 V  

**Noise Margins:**  
- NMH = 1.8 − 1.148 = **0.652 V**  
- NML = 0.728 − 0 = **0.728 V**

---

### **Case 4 — (Wp/Wn = 4)**  
Wn = 0.195 µm, Wp = 0.78 µm

**VTC Plot:**  
<img width="632" height="684" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />

**From Graph:**  
- Vm = 1.01 V  
- Voh = 1.8 V, Vol = 0 V  
- Vil = 0.851 V, Vih = 1.20 V  

**Noise Margins:**  
- NMH = 1.8 − 1.20 = **0.60 V**  
- NML = 0.851 − 0 = **0.851 V**

---

### **Case 5 — (Wp/Wn = 5)**  
Wn = 0.195 µm, Wp = 0.975 µm

**VTC Plot:**  
<img width="632" height="684" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

**From Graph:**  
- Vm = 1.06 V  
- Voh = 1.8 V, Vol = 0 V  
- Vil = 0.897 V, Vih = 1.31 V  

**Noise Margins:**  
- NMH = 1.8 − 1.31 = **0.49 V**  
- NML = 0.897 − 0 = **0.897 V**

---

## 📊 Summary Table

| Case | (Wp/Wn) | Vm (V) | Vil (V) | Vih (V) | NMH (V) | NML (V) |
|------|----------|--------|---------|---------|---------|---------|
| 1 | 1 | 0.70 | 0.375 | 0.820 | 0.98 | 0.375 |
| 2 | 2 | 0.86 | 0.632 | 1.018 | 0.782 | 0.632 |
| 3 | 3 | 0.95 | 0.728 | 1.148 | 0.652 | 0.728 |
| 4 | 4 | 1.01 | 0.851 | 1.20 | 0.60 | 0.851 |
| 5 | 5 | 1.06 | 0.897 | 1.31 | 0.49 | 0.897 |

---

## 🧠 Observations

- As **Wp/Wn** increases (PMOS becomes stronger relative to NMOS):
  - The **switching threshold (Vm)** increases.
  - **NML** (Noise Margin for logic ‘0’) increases.
  - **NMH** (Noise Margin for logic ‘1’) decreases.
- The inverter becomes more **biased toward the high state**, as the pull-up network dominates.
- To achieve **symmetrical noise margins**, select a Wp/Wn ratio that yields Vm ≈ VDD/2.  
  → In this dataset, that occurs around **Case 2 – Case 3 (Vm ≈ 0.9 V)**.

---

## 🏁 Conclusion

The corrected dataset shows how increasing PMOS width affects inverter balance and noise margins.  
Case 2 – 3 provides the most balanced operation for a 1.8 V supply.

---

**Author:** *Chippagiri Vamsi Krishna*  
**Tool:** *NGSPICE (130 nm Sky130)*



