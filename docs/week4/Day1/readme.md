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




NMOS Current–Voltage (I 
D
​
 –V 
DS
​
 ) Relationship in the Linear (Resistive) Region
1. Region Definition
Transistor Type: N-channel MOSFET (NMOS)

Region: Resistive / Linear / Triode Region

Conditions:

V 
GS
​
 >V 
t
​
  (Transistor ON, strong inversion)

V 
DS
​
 <(V 
GS
​
 −V 
t
​
 ) (Channel exists fully from Source to Drain)

V 
DS
​
  is small

Example:

V 
GS
​
 =1V, V 
DS
​
 =0.05V, V 
t
​
 =0.45V

2. Channel Charge and Current Flow
(a) Channel Charge (Q 
i
​
 (x))
At a distance x from the Source, potential is V(x). The charge is negative (due to electrons).

Q 
i
​
 (x)=−C 
ox
​
 [(V 
GS
​
 −V(x))−V 
t
​
 ]

(C 
ox
​
 : Oxide capacitance per unit area)

(b) Electron Velocity
The velocity is proportional to the electric field ( 
dx
dV
​
 ):

v 
n
​
 (x)=−μ 
n
​
  
dx
dV
​
 

(μ 
n
​
 : Electron mobility)

(c) Drain Current (I 
D
​
 )
The current is the flow of charge across the channel width W:

I 
D
​
 =W⋅Q 
i
​
 (x)⋅v 
n
​
 (x)

Substituting Q 
i
​
 (x) and v 
n
​
 (x):

I 
D
​
 =μ 
n
​
 C 
ox
​
 W[(V 
GS
​
 −V(x))−V 
t
​
 ] 
dx
dV
​
 
3. Integration Along the Channel
Integrate I 
D
​
 ⋅dx from x=0 to x=L and the voltage terms from V=0 (Source) to V=V 
DS
​
  (Drain):

I 
D
​
 ∫ 
0
L
​
 dx=μ 
n
​
 C 
ox
​
 W∫ 
0
V 
DS
​
 
​
 [(V 
GS
​
 −V 
t
​
 )−V]dV

Solving the integral, we get the fundamental equation:

I 
D
​
 = 
L
μ 
n
​
 C 
ox
​
 W
​
 [(V 
GS
​
 −V 
t
​
 )V 
DS
​
 − 
2
V 
DS
2
​
 
​
 ]
4. Simplified Form
Define constants:

k 
n
′
​
 =μ 
n
​
 C 
ox
​
 andk 
n
​
 =k 
n
′
​
  
L
W
​
 (Gain factor)

Final Equation for the Resistive Region:

I 
D
​
 =k 
n
​
 [(V 
GS
​
 −V 
t
​
 )V 
DS
​
 − 
2
V 
DS
2
​
 
​
 ]
​
 
5. Linear Approximation (for Very Small V 
DS
​
 )
If V 
DS
​
  is very small, the quadratic term  
2
V 
DS
2
​
 
​
  is negligible compared to the linear term (V 
GS
​
 −V 
t
​
 )V 
DS
​
 .
The equation simplifies to a linear relationship:

I 
D
​
 ≈k 
n
​
 (V 
GS
​
 −V 
t
​
 )V 
DS
​
 

⇒ The transistor behaves like a voltage-controlled resistor, with resistance inversely proportional to (V 
GS
​
 −V 
t
​
 ).

6. Example Calculation
Given: V 
GS
​
 =1V,V 
DS
​
 =0.05V,V 
t
​
 =0.45V

I 
D
​
 =k 
n
​
 [(1−0.45)(0.05)− 
2
(0.05) 
2
 
​
 ]
I 
D
​
 =k 
n
​
 [0.0275−0.00125]=k 
n
​
 (0.02625)

The quadratic term (0.00125) is small compared to the linear term (0.0275), validating the linear approximation.


