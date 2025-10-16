# Static Behaviour Evalution - Switching Threshold(Vm)

Spice simulation is done using 65nm technology model file. This model file is in Day3 folder.

## case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

### Netlist : 
```
.include cmos_65nm.txt

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

### Plot : VTC Curve 

<img width="633" height="661" alt="image" src="https://github.com/user-attachments/assets/6dc634f0-ed3e-449c-b0f1-f09b556e3be6" />

