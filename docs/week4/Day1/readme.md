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
