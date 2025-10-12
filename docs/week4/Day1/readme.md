# Circuit design

##  **Basic Circuit Element** : NMOS

---

### **1. NMOS – Basics**

* **NMOS (N-channel MOSFET)** is a **voltage-controlled switch**.
* **Structure:**

  * Two **n+ regions** (Source & Drain) in a **p-type substrate**.
  * **Gate** is separated by a thin **oxide layer**.
* **Working:**

  * Current flows between **Source (S)** and **Drain (D)** when a voltage is applied at **Gate (G)**.
  * At **$V_{GS} = 0$**, no channel exists → **Transistor OFF** (high resistance).

---

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

---

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


---

### 4. NMOS Current–Voltage (𝐼ᴅ–Vᴅs) Relationship in the Linear (Resistive) Region

#### Linear Region

**Transistor Type:** N-channel MOSFET (NMOS)  
**Region:** Resistive / Linear / Triode Region  

**Conditions:**

* $V_{GS} > V_t$ → Transistor **ON** (strong inversion)
* $V_{DS} < (V_{GS} - V_t)$ → Channel exists fully from Source to Drain
* $V_{DS}$ is **small**

**Example:**  
$V_{GS} = 1\,\text{V},\; V_{DS} = 0.05\,\text{V},\; V_t = 0.45\,\text{V}$


#### Channel Charge and Current Flow

##### (a) **Channel Charge ($Q_i(x)$)**

At a distance **x** from the Source, potential = $V(x)$.

$$
Q_i(x) = -C_{ox} \big[(V_{GS} - V(x)) - V_t \big]
$$

* $C_{ox}$: Oxide capacitance per unit area  
* The charge is **negative** (due to electrons).


##### (b) **Electron Velocity**

$$
v_n(x) = -\mu_n \frac{dV}{dx}
$$

* $\mu_n$: Electron mobility  
* $\frac{dV}{dx}$: Electric field along the channel  


##### (c) **Drain Current ($I_D$)**

$$
I_D = W \cdot Q_i(x) \cdot v_n(x)
$$

Substitute $Q_i(x)$ and $v_n(x)$:

$$
I_D = \mu_n C_{ox} W \big[(V_{GS} - V(x)) - V_t \big] \frac{dV}{dx}
$$



#### Integration Along the Channel**

Integrate from Source (x=0, V=0) to Drain (x=L, V=V_{DS}):

$$
I_D = \frac{\mu_n C_{ox} W}{L} \int_0^{V_{DS}} \big[(V_{GS} - V) - V_t \big] dV
$$

$$
I_D = \frac{\mu_n C_{ox} W}{L} \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2} \Big]
$$


#### Simplified Form**

Define constants:

$$
k_n' = \mu_n C_{ox} \quad \text{and} \quad k_n = k_n' \frac{W}{L}
$$

**Final Equation:**

$$
\boxed{I_D = k_n \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2}\Big]}
$$



#### Linear Approximation (for Very Small $V_{DS}$)**

If $V_{DS}$ is **very small**,  
the quadratic term $\frac{V_{DS}^2}{2} \ll (V_{GS} - V_t)V_{DS}$.

So,

$$
I_D \approx k_n (V_{GS} - V_t)V_{DS}
$$

 **Transistor behaves like a resistor**, with resistance controlled by $V_{GS}$.



#### Example Calculation**

Given:  
$V_{GS}=1\,\text{V},\; V_{DS}=0.05\,\text{V},\; V_t=0.45\,\text{V}$

$$
I_D = k_n \Big[(1 - 0.45)(0.05) - \frac{(0.05)^2}{2}\Big]
$$

$$
I_D = k_n [0.0275 - 0.00125] = k_n (0.02625)
$$

Since the second term is **small**, linear approximation is valid.

