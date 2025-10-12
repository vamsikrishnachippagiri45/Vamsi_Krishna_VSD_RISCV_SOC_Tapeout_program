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

##### (a) **Channel Charge ($$Q_i(x)$$)**

At a distance **x** from the Source, potential = $V(x)$.

$$
Q_i(x) = -C_{ox} \big[(V_{GS} - V(x)) - V_t \big]
$$

* $C_{ox}$: Oxide capacitance per unit area  
* The charge is **negative** (due to electrons).
* $$\big[(V_{GS} - V(x)) \big]$$ is the effective gate voltage at a point x along channel. (At source(x=0) -> V(x)=0 , and At Drain(x=L) -> V(x)=Vds )


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



#### Integration Along the Channel

Integrate from Source (x=0, V=0) to Drain (x=L, V=V_{DS}):

$$
I_D = \frac{\mu_n C_{ox} W}{L} \int_0^{V_{DS}} \big[(V_{GS} - V) - V_t \big] dV
$$

$$
I_D = \frac{\mu_n C_{ox} W}{L} \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2} \Big]
$$


#### Simplified Form

Define constants:

$$
k_n' = \mu_n C_{ox} \quad \text{and} \quad k_n = k_n' \frac{W}{L}
$$

**Final Equation:**

$$
\boxed{I_D = k_n \Big[(V_{GS} - V_t)V_{DS} - \frac{V_{DS}^2}{2}\Big]}
$$



#### Linear Approximation (for Very Small $V_{DS}$)

If $V_{DS}$ is **very small**,  
the quadratic term $\frac{V_{DS}^2}{2} \ll (V_{GS} - V_t)V_{DS}$.

So,

$$
I_D \approx k_n (V_{GS} - V_t)V_{DS}
$$

 **Transistor behaves like a resistor**, with resistance controlled by $V_{GS}$.



#### Example Calculation

Given:  
$V_{GS}=1\,\text{V},\; V_{DS}=0.05\,\text{V},\; V_t=0.45\,\text{V}$

$$
I_D = k_n \Big[(1 - 0.45)(0.05) - \frac{(0.05)^2}{2}\Big]
$$

$$
I_D = k_n [0.0275 - 0.00125] = k_n (0.02625)
$$

Since the second term is **small**, linear approximation is valid.

---



### 5. NMOS: Transition from Linear (Resistive) Region to Saturation — Pinch-Off & Channel-Length Modulation

#### Overview
As the drain-to-source voltage \(V_{DS}\) increases, an NMOS operating in strong inversion transitions from the **Linear (Resistive/Triode)** region into the **Saturation** region. The transition is governed by the **pinch-off** phenomenon at the drain end of the channel.



#### Pinch-Off Condition

- The local effective gate-to-channel voltage at position \(x\) is:
  
$$
V_{GC}(x) = V_{GS} - V(x)
$$
  
where \(V(x)\) is the local channel potential (0 at source, \(V_{DS}\) at drain).

- In the **Linear** region, \(V_{DS}\) is small and the channel remains inverted along its entire length:

$$
V_{GD} = V_{GS} - V_{DS} \quad\text{and}\quad V_{GD} > V_t
$$

- **Pinch-off (Saturation Boundary)** occurs when the gate-to-channel voltage at the drain end equals the threshold:
  
$$
V_{GS} - V_{DS,sat} = V_t
$$
  
  therefore
  
$$
\boxed{V_{DS,sat} = V_{GS} - V_t}
$$

**Interpretation (example):** for $$\(V_{GS}=1\text{ V}\)$$ and $$\(V_t=0.45\text{ V}\)$$ :

$$\(V_{DS}<0.55\text{ V}\)$$→ linear (channel continuous)
  
$$\(V_{DS}=0.55\text{ V}\)$$ → pinch-off onset (boundary)
  
$$\(V_{DS}>0.55\text{ V}\)$$ → saturation (pinch-off region at drain)



#### What Physically Happens at Pinch-Off

- As \(V_{DS}\) rises, the local potential \(V(x)\) near the drain approaches \(V_{GS} - V_t\), reducing \(V_{GC}\) there.
- When \(V_{GC}\) at drain = \(V_t\), the inversion layer is just removed at the drain end → **pinch-off**.
- After pinch-off, the channel does not extend fully to the drain: a small **depletion/pinch-off region** forms near the drain.
- **Current flow continues** because carriers are injected from the channel edge and accelerated across the strong electric field in the pinch-off region to the drain terminal.
- **Result:** increasing \(V_{DS}\) beyond \(V_{DS,sat}\) does **not** appreciably increase the current in the ideal model → transistor behaves like a **(nearly) constant current source**.



#### Ideal First-Order Saturation Current (Pinch-Off Model)

Using the standard square-law, the ideal (first-order) saturation current is:

$$
\boxed{I_D = \tfrac{1}{2}\,k_n\,(V_{GS} - V_t)^2}
$$

where
$$
k_n = \mu_n C_{ox}\,\frac{W}{L} \quad\text{(gain factor)}
$$

This expression assumes:
- Long-channel device (no CLM)
- Mobility constant (no velocity saturation)
- No series resistances or bulk effects



#### Channel-Length Modulation 

**Why ideal current is not perfectly constant:**  
When \(V_{DS}\) increases beyond \(V_{DS,sat}\), the pinch-off point moves slightly from the drain toward the source. This shortens the **effective channel length** \(L'\) (i.e., \(L' < L\)). A shorter channel increases the current (because \(k_n \propto W/L'\)), so \(I_D\) grows weakly with \(V_{DS}\).

**Phenomenological model (introduce \(\lambda\)):**

$$
\boxed{I_D = \tfrac{1}{2}\,k_n\,(V_{GS} - V_t)^2\,(1 + \lambda\,V_{DS})}
$$

- \(\lambda\) = channel-length modulation parameter (units: V\(^{-1}\))
- For small \(\lambda\), the device is **almost** an ideal current source; larger \(\lambda\) means stronger dependence on \(V_{DS}\).

**Interpretation of \(\lambda\):**
- \(\lambda \approx \frac{1}{L_{\text{eff}}}\)-like scaling (shorter devices → larger \(\lambda\))
- Extracted from measurements by fitting \(I_D\) vs \(V_{DS}\) in saturation.



#### Putting It Together — Region Conditions & Key Equations

**Region conditions:**
- **Cutoff:** \(V_{GS} < V_t\)  
  $$I_D \approx 0$$

- **Linear (Resistive/Triode):** \(V_{GS} > V_t\) and \(V_{DS} < V_{GS} - V_t\)  
  $$I_D = k_n\Big[(V_{GS}-V_t)V_{DS} - \tfrac{V_{DS}^2}{2}\Big]$$  
  (For very small \(V_{DS}\): \(I_D \approx k_n (V_{GS}-V_t) V_{DS}\).)

- **Saturation (pinch-off):** \(V_{DS} \ge V_{GS} - V_t\)  
  *Ideal:*
  $$I_D = \tfrac{1}{2} k_n (V_{GS} - V_t)^2$$  
  *With CLM:*
  $$I_D = \tfrac{1}{2} k_n (V_{GS} - V_t)^2 (1 + \lambda V_{DS})$$


#### Note

- **Channel-Length Modulation** is analogous to the Early effect in BJTs: it creates finite output conductance in saturation.
- **Velocity Saturation (high field)**: in deep submicron processes, carrier velocity saturates and the square-law becomes invalid; saturation current becomes closer to linear with \(V_{GS}-V_t\).
- **Body Effect / Substrate Bias:** changing \(V_{SB}\) modifies \(V_t\) (body effect), which shifts \(V_{DS,sat}\) and all \(I_D\) expressions.
- **Short-channel effects** (DIBL, mobility degradation, series resistances) further modify the simple models and typically require process-specific parameters.



#### Example

Let $\(V_{GS}=1.0\ \text{V}\), \(V_t=0.45\ \text{V}\)$:  
- $\(V_{DS,sat}=V_{GS}-V_t=0.55\ \text{V}\)$.  
- For $\(V_{DS}=0.05\ \text{V}\)$ → Linear region (resistive).  
- For $\(V_{DS}=0.65\ \text{V}\)$ → Saturation (pinch-off established).  
- If $\(\lambda>0\), \(I_D\)$ will slowly increase with $\(V_{DS}\)$ in saturation.

---













