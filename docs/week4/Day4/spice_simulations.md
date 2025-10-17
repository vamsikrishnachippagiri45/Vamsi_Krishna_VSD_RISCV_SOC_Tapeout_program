

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

# 📘 CMOS Inverter – Static Behaviour Evaluation (Corrected Wp / Wn)

> **Note:** previous version had `Wp` and `Wn` labels interchanged. This file uses the corrected device sizing:
> - \(W_n = 0.195\ \mu m\) (constant)
> - \(W_p =\) 0.195, 0.39, 0.585, 0.78, 0.975 µm
> - Thus \(\dfrac{W_p}{W_n} = 1, 2, 3, 4, 5\)

---

## ⚙️ Simulation Parameters (summary)

- Technology: **130 nm**  
- \(V_{DD} = 1.8\ \text{V}\)  
- \(L_n = L_p = 65\ \text{nm}\)  
- Tool: **NGSPICE** (Sky130 models)  
- Input sweep: 0 → 1.8 V

---

## Case-by-case VTC results (corrected)

### **Case 1 — \(W_n = 0.195\mu m,\; W_p = 0.195\mu m\) → \(W_p/W_n = 1\)**

**VTC plot:**  
<img width="551" height="620" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />

- \(V_M = 0.70\ \text{V}\)  
- \(V_{OH} = 1.8\ \text{V},\; V_{OL} = 0\ \text{V}\)  
- \(V_{IL} = 0.375\ \text{V},\; V_{IH} = 0.820\ \text{V}\)

**Noise Margins:**
- \(NM_H = V_{OH} - V_{IH} = 1.8 - 0.820 = 0.98\ \text{V}\)  
- \(NM_L = V_{IL} - V_{OL} = 0.375 - 0 = 0.375\ \text{V}\)

---

### **Case 2 — \(W_n = 0.195\mu m,\; W_p = 0.39\mu m\) → \(W_p/W_n = 2\)**

**VTC plot:**  
<img width="585" height="640" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

- \(V_M = 0.86\ \text{V}\)  
- \(V_{OH} = 1.8\ \text{V},\; V_{OL} = 0\ \text{V}\)  
- \(V_{IL} = 0.632\ \text{V},\; V_{IH} = 1.018\ \text{V}\)

**Noise Margins:**
- \(NM_H = 1.8 - 1.018 = 0.782\ \text{V}\)  
- \(NM_L = 0.632 - 0 = 0.632\ \text{V}\)

---

### **Case 3 — \(W_n = 0.195\mu m,\; W_p = 0.585\mu m\) → \(W_p/W_n = 3\)**

**VTC plot:**  
<img width="627" height="684" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />

- \(V_M = 0.95\ \text{V}\)  
- \(V_{OH} = 1.8\ \text{V},\; V_{OL} = 0\ \text{V}\)  
- \(V_{IL} = 0.728\ \text{V},\; V_{IH} = 1.148\ \text{V}\)

**Noise Margins:**
- \(NM_H = 1.8 - 1.148 = 0.652\ \text{V}\)  
- \(NM_L = 0.728 - 0 = 0.728\ \text{V}\)

---

### **Case 4 — \(W_n = 0.195\mu m,\; W_p = 0.78\mu m\) → \(W_p/W_n = 4\)**

**VTC plot:**  
<img width="632" height="684" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />

- \(V_M = 1.01\ \text{V}\)  
- \(V_{OH} = 1.8\ \text{V},\; V_{OL} = 0\ \text{V}\)  
- \(V_{IL} = 0.851\ \text{V},\; V_{IH} = 1.20\ \text{V}\)

**Noise Margins:**
- \(NM_H = 1.8 - 1.20 = 0.60\ \text{V}\)  
- \(NM_L = 0.851 - 0 = 0.851\ \text{V}\)

---

### **Case 5 — \(W_n = 0.195\mu m,\; W_p = 0.975\mu m\) → \(W_p/W_n = 5\)**

**VTC plot:**  
<img width="632" height="684" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

- \(V_M = 1.06\ \text{V}\)  
- \(V_{OH} = 1.8\ \text{V},\; V_{OL} = 0\ \text{V}\)  
- \(V_{IL} = 0.897\ \text{V},\; V_{IH} = 1.31\ \text{V}\)

**Noise Margins:**
- \(NM_H = 1.8 - 1.31 = 0.49\ \text{V}\)  
- \(NM_L = 0.897 - 0 = 0.897\ \text{V}\)

---

## 📊 Summary Table (corrected)

| Case | \(W_p/W_n\) | \(V_M\) (V) | \(V_{IL}\) (V) | \(V_{IH}\) (V) | \(NM_H\) (V) | \(NM_L\) (V) |
|------|-------------:|------------:|---------------:|---------------:|-------------:|-------------:|
| 1 | 1 | 0.70 | 0.375 | 0.820 | 0.98 | 0.375 |
| 2 | 2 | 0.86 | 0.632 | 1.018 | 0.782 | 0.632 |
| 3 | 3 | 0.95 | 0.728 | 1.148 | 0.652 | 0.728 |
| 4 | 4 | 1.01 | 0.851 | 1.20  | 0.60  | 0.851 |
| 5 | 5 | 1.06 | 0.897 | 1.31  | 0.49  | 0.897 |

---

## 🔍 Observations (corrected)

- **As \(W_p/W_n\) increases (PMOS gets stronger relative to NMOS):**
  - The **switching threshold \(V_M\)** **increases** (moves toward/above mid-supply in these simulations).
  - **Noise Margin Low (NM\_L)** **increases** (improved tolerance for low-level noise).
  - **Noise Margin High (NM\_H)** **decreases** (reduced tolerance for high-level noise).
- This trade-off is expected: making PMOS stronger shifts the balance so the inverter holds output high better (raising \(V_M\) and NML) while reducing the headroom for robust highs (reducing NMH).
- **Goal for symmetric noise margins**: choose \(W_p/W_n\) so that \(V_M \approx V_{DD}/2\). In this dataset, the best match to \(V_{DD}/2 = 0.9\) V appears around **Case 2–3** (VM = 0.86–0.95 V).

---

## 🧾 Conclusion

- The corrected dataset and table now reflect **real device sizing** where \(W_p\) increases while \(W_n\) is constant.  
- Use these results to pick a PMOS width that gives the desired trade-off between NMH and NML depending on your noise/immunity priorities.

---

**Author:** Chippagiri Vamsi Krishna  
**Tool:** NGSPICE (Sky130)  



- The **switching threshold** of a CMOS inverter depends on the $\frac{W_p}{W_n}$ ratio.  
- For **balanced transfer characteristics**, $V_M$ should be around $V_{DD}/2$.  
- The study confirms that **increasing PMOS width** (higher $\frac{W_p}{W_n}$) improves **NMH** but reduces **NML**.

