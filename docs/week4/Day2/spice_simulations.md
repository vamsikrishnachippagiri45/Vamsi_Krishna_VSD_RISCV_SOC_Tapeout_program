# Spice simulations 

## 1) Long channel MOSFET ( W=1.8um , L=1.2um , W/L = 1.5)

### Spice Netlist for Id Vs Vds:

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=1.8 l=1.2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```
### Plot : Id Vs Vds 

<img width="1243" height="781" alt="image" src="https://github.com/user-attachments/assets/2ccbe7eb-9f34-4774-9d26-728567bb45b8" />

### Spice Netlist for Id Vs Vgs:

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=1.8 l=1.2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.1 Vdd 0 1.8 1.8

.control
run
display
setplot dc1
.endc

.end
```

### Plot : Id Vs Vgs  at Vds = 1.8V

<img width="1147" height="780" alt="image" src="https://github.com/user-attachments/assets/1985b759-ee01-455a-8c85-2b12d2ee4de8" />

### Observations
- At low Vds, Id rises with Vds quadratically (triode/ohmic region).  
- Saturation occurs at higher Vds via pinch-off; Id plateaus with strong dependence on Vgs (Id ∝ (Vgs − Vt)²).  
- Output conductance in saturation is small (good saturation behavior).  
- Id vs Vgs at Vds = 1.8 V shows a **quadratic increase** with Vgs, confirming long-channel behavior.

---

## 1) Short channel MOSFET ( W=0.375um , L=0.25um , W/L = 1.5)

### Spice Netlist for Id Vs Vds:

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.375 l=0.25
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```
### Plot : Id Vs Vds 

<img width="1283" height="791" alt="image" src="https://github.com/user-attachments/assets/8e178caa-256c-4917-bf02-b274802f3e88" />


### Spice Netlist for Id Vs Vgs:

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.375 l=0.25
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.1 Vdd 0 1.8 1.8

.control
run
display
setplot dc1
.endc

.end
```

### Plot : Id Vs Vgs  at Vds = 1.8V

<img width="1059" height="755" alt="image" src="https://github.com/user-attachments/assets/08a98929-e980-426c-925e-8a64e6d50bc9" />


### Observations
- Initial quadratic rise with Vds is very short; curves flatten early, indicating early onset of velocity saturation.  
- Saturation current is lower than the long-channel device for the same W/L.  
- Id in the saturation region shows approximately **linear dependence on Vgs** (Id ∝ Vgt), consistent with velocity-limited behavior.  
- Output conductance is higher in saturation than long-channel, showing reduced saturation quality.  
- Id vs Vgs at Vds = 1.8 V shows a **linear increase** with Vgs, characteristic of short-channel velocity saturation.

---

## Comparison: Long-Channel vs Short-Channel MOSFETs

| Characteristic | Long-Channel MOSFET (W = 1.8 µm, L = 1.2 µm) | Short-Channel MOSFET (W = 0.375 µm, L = 0.25 µm) | Notes / Physical Explanation |
|----------------|-----------------------------------------------|--------------------------------------------------|------------------------------|
| **Triode/Linear Region (Low Vds)** | Id rises quadratically with Vds and Vgs (Id ∝ (Vgs − Vt)·Vds − Vds²/2). | Initial quadratic rise is very small; Id quickly becomes linear with Vds. | Short channel → higher electric field along the channel → early velocity saturation. |
| **Saturation Onset (Vdsat)** | Occurs at higher Vds, determined by pinch-off condition Vdsat ≈ Vgs − Vt. | Occurs at smaller Vds, determined by velocity saturation: Vdsat ≈ Ec · L. | In short-channel devices, the channel is so short that even moderate Vds produces a high electric field exceeding Ec. |
| **Id vs Vgs in Saturation** | Quadratic increase with Vgs (Id ∝ (Vgs − Vt)²). | Linear increase with Vgs (Id ∝ Vgt), reflecting v_sat-limited current. | Velocity saturation dominates in short-channel devices, limiting carrier velocity and making Id proportional to Vgt rather than Vgt². |
| **Peak Drain Current (Id,max)** | Higher peak Id for same W/L ratio. | Lower peak Id for same W/L ratio. | Velocity saturation limits the maximum carrier velocity in short-channel devices, reducing current. |
| **Output Conductance (g_ds)** | Low in saturation (flatter Id vs Vds plateau). | Higher in saturation (Id vs Vds plateau has noticeable slope). | Higher g_ds in short-channel devices indicates less ideal saturation due to velocity-limited behavior. |
| **Transconductance (gm = dId/dVgs)** | High in saturation (steep Id vs Vgs curve). | Reduced in saturation (less steep Id vs Vgs curve). | Reduced gm in short-channel devices is due to velocity saturation limiting current increase with Vgs. |
| **Saturation Mechanism** | Pinch-off: channel charge at the drain end goes to zero. | Velocity saturation: carrier velocity limited to v_sat due to high electric field. | Fundamental difference in physics between long- and short-channel devices. |
| **Id vs Vds Shape** | Clear quadratic-to-flat transition (triode to pinch-off). | Rapid transition from initial rise to nearly linear plateau (triode to velocity saturation). | Short-channel effect modifies the curve early; the flat region is less “perfect.” |
| **Effect of Channel Length (L)** | Longer channel → lower electric field → pinch-off occurs before velocity saturation. | Short channel → higher electric field → velocity saturation dominates before pinch-off. | Shows why short-channel devices behave differently even with same W/L ratio. |

**Summary :**
- Long-channel MOSFETs: Classic quadratic Id–Vgs and clear pinch-off saturation; good saturation with low g_ds.  
- Short-channel MOSFETs: Early saturation due to velocity-limited carriers; linear Id–Vgs in saturation; reduced peak current, higher output conductance, and lower transconductance.  
- Short-channel devices require **velocity saturation models** for accurate current prediction, unlike long-channel devices where the quadratic model is sufficient.
