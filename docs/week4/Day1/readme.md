# Circuit design

##  **Basic Circuit Element** : NMOS


### **1. NMOS – Basics**

* **NMOS (N-channel MOSFET)** is a **voltage-controlled switch**.
* **Structure:**

  * Two **n+ regions** (Source & Drain) in a **p-type substrate**.
  * **Gate** is separated by a thin **oxide layer**.
* **Working:**

  * Current flows between **Source (S)** and **Drain (D)** when a voltage is applied at **Gate (G)**.
  * At **$V_{GS} = 0$**, no channel exists → **Transistor OFF** (high resistance).


### **2. Strong Inversion & Threshold Voltage ($V_t$)**

* When **Gate voltage ($V_{GS}$)** increases:

  * Electrons are attracted toward the Gate area.
  * A **depletion region** forms first.
  * At a certain $V_{GS}$, enough electrons accumulate → **n-channel forms**.
* This condition is called **Strong Inversion**.
* The **voltage at which this happens** is called **Threshold Voltage ($V_t$)**.

**Behavior:**

* If **$V_{GS} < V_t$ → OFF (no conduction)**
* If **$V_{GS} ≥ V_t$ → ON (channel formed, current flows)**



### **3. Body (Substrate) Effect**

When $V_{SB} \neq 0$ (Source is at a higher potential than Bulk):
The Source becomes positive with respect to the Bulk (Substrate).
This increases the reverse bias of the p–n junction between Source (n+) and Bulk (p).

Due to this stronger reverse bias:
More holes are repelled from the channel region.
The depletion region widens under the Gate.
So, fewer electrons are available near the surface to form the n-channel.

Because the Source is positive, it tends to attract the channel electrons downward, away from the inversion layer.
Therefore, the Gate must apply a higher positive voltage ($V_{GS}$) to pull enough electrons toward the surface and re-form the inversion layer.

The **Threshold Voltage Equation** expresses this relationship: $$V_t = V_{t0} + \gamma (\sqrt{|-2\Phi_f + V_{sb}|} - \sqrt{|-2\Phi_f|})$$

| Symbol   | Meaning                         |
| :------- | :------------------------------ |
| $V_t$    | New threshold voltage           |
| $V_{t0}$ | Threshold voltage at $V_{SB}=0$ |
| $\gamma$ | Body effect coefficient         |
| $V_{SB}$ | Source-to-Bulk voltage          |
| $\Phi_f$ | Fermi potential                 |

**Summary:**

* **$V_{GS}$ controls NMOS conduction.**
* **$V_t$** = minimum voltage needed to form channel.
* **Body Effect:** Higher $V_{SB}$ → Higher $V_t$ → NMOS harder to turn ON.











Perfect 👍 Here’s the **complete version** — with both **theory and equations** in **GitHub-ready Markdown format** (using LaTeX blocks).
You can copy-paste this directly into your `.md` file.

---

## 🧠 NMOS, Strong Inversion & Threshold Voltage

### **1. NMOS – Basic Concept**

The **N-channel MOSFET (NMOS)** acts as a **voltage-controlled switch**.

* **Structure:**
  Two **n⁺ regions** (Source & Drain) in a **p-type substrate**, with a **Gate** separated by a thin **oxide** layer.

* **Working:**
  Current flows between **Source (S)** and **Drain (D)** when a voltage is applied at **Gate (G)**.
  At ( V_{GS} = 0 ), no channel exists → **Transistor OFF**.

---

### **2. Strong Inversion & Threshold Voltage**

As ( V_{GS} ) increases:

* Electrons are attracted toward the Gate area.
* A **depletion region** forms beneath the oxide.
* At a certain ( V_{GS} ), enough electrons accumulate to form an **n-channel** → transistor turns **ON**.

The voltage at which this occurs is the **Threshold Voltage ( V_t )**.

| Condition          | Transistor State    |
| :----------------- | :------------------ |
| ( V_{GS} < V_t )   | OFF (no channel)    |
| ( V_{GS} \ge V_t ) | ON (channel formed) |

---

### **3. Body (Substrate) Effect**

When the **Source** is at a higher potential than the **Bulk** (( V_{SB} > 0 )):

* The **p–n junction** between Source and Bulk becomes more **reverse biased**.
* The **depletion region widens**, repelling more holes.
* Fewer electrons remain near the surface → **harder to invert**.
* Thus, the **Gate must apply a higher voltage** to form the same channel.

Hence, the **Threshold Voltage increases** with ( V_{SB} ):

```latex
$$V_t = V_{t0} + \gamma \left(\sqrt{|2\Phi_f + V_{SB}|} - \sqrt{|2\Phi_f|}\right)$$
```

Where:

| Symbol   | Meaning                         |
| :------- | :------------------------------ |
| (V_t)    | New threshold voltage           |
| (V_{t0}) | Threshold voltage at (V_{SB}=0) |
| (\gamma) | Body-effect coefficient         |
| (V_{SB}) | Source-to-Bulk voltage          |
| (\Phi_f) | Fermi potential                 |

---

## ⚡ NMOS ( I_D - V_{DS} ) Relationship (Linear / Resistive Region)

### **1. Operating Region**

**Condition for Linear Region:**

```latex
$$V_{GS} > V_t \quad \text{and} \quad V_{DS} < (V_{GS} - V_t)$$
```

Here, the transistor conducts and behaves like a **voltage-controlled resistor**.

---

### **2. Channel Charge and Current Flow**

At a distance **x** from the Source:

```latex
$$Q_i(x) = -C_{ox}\big[(V_{GS} - V(x)) - V_t\big]$$
```

* ( Q_i(x) ): Inversion charge per unit area
* ( C_{ox} ): Gate oxide capacitance per unit area

Electron drift velocity:

```latex
$$v_n(x) = -\mu_n \frac{dV}{dx}$$
```

Drain current at that point:

```latex
$$I_D = \mu_n C_{ox} W \big[(V_{GS} - V(x)) - V_t\big] \frac{dV}{dx}$$
```

---

### **3. Integration Along the Channel**

Integrate from **Source (x=0, V=0)** to **Drain (x=L, V=V_{DS})**:

```latex
$$I_D = \frac{\mu_n C_{ox} W}{L} \int_0^{V_{DS}} \big[(V_{GS} - V) - V_t\big]\,dV$$
```

After integration:

```latex
$$I_D = \frac{\mu_n C_{ox} W}{L} \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2}\Big]$$
```

---

### **4. Simplified with Process Parameters**

Define:

```latex
$$k_n' = \mu_n C_{ox} \quad \text{and} \quad k_n = k_n' \frac{W}{L}$$
```

So,

```latex
$$I_D = k_n \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2}\Big]$$
```

---

### **5. Linear Approximation (for Small (V_{DS}))**

If ( V_{DS} ) is very small:

```latex
$$I_D \approx k_n (V_{GS} - V_t)V_{DS}$$
```

➡️ The transistor behaves like a **resistor** controlled by ( V_{GS} ).

---

### **6. Saturation Region**

When ( V_{DS} \ge (V_{GS} - V_t) ):


$$I_D = \frac{1}{2}k_n (V_{GS} - V_t)^2$$





