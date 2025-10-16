# Static Behaviour Evalution - Switching Threshold(Vm)

Spice simulation is done using 130nm technology model file. This model file is in Day3 folder.

# case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

## Netlist for VTC: 
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

## Plot : VTC Curve 

<img width="633" height="661" alt="image" src="https://github.com/user-attachments/assets/6dc634f0-ed3e-449c-b0f1-f09b556e3be6" />

From graph , Vm = 0.65V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.195um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1ns 10ns 

.control
run
.endc

.end
```

<img width="942" height="659" alt="image" src="https://github.com/user-attachments/assets/28fa63e7-e240-4a8f-a8e7-0c9c5822e95f" />

## plot : transient analysis

<img width="821" height="574" alt="image" src="https://github.com/user-attachments/assets/cb90748b-989d-425b-94a9-c950f39d3c13" />

Rise time : 0.368ns

Fall time : 0.180ns
