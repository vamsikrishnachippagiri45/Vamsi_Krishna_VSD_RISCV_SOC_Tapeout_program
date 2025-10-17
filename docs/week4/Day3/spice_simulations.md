# Static Behaviour Evalution - Switching Threshold(Vm)

Spice simulation is done using 130nm technology model file. This model file is in Day3 folder.

# Case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

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

Rise time = 0.368ns

Fall time = 0.180ns

---

# Case 2 : Wn=0.195um,Wp=0.39um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 2 ( $\frac{Wp}{Lp}$ )

## Netlist for VTC: 
```
.include cmos_65nm.txt

M1 out in vdd vdd pmos w=0.39um l=0.13um
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

## Plot : VTC Curve 

<img width="657" height="685" alt="image" src="https://github.com/user-attachments/assets/aed51dac-de00-43a3-9d50-74e4144b0c0d" />

From graph , Vm = 0.75V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.39um l=130nm
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

## plot : transient analysis

<img width="835" height="640" alt="image" src="https://github.com/user-attachments/assets/74b89a9b-2597-4313-9578-cc0284482600" />


Rise time = 0.194ns

Fall time = 0.173ns

---

# Case 3 : Wn=0.195um,Wp=0.585um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 3 ( $\frac{Wp}{Lp}$ )

## Netlist for VTC: 
```
.include cmos_65nm.txt

M1 out in vdd vdd pmos w=0.585um l=0.13um
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

## Plot : VTC Curve 

<img width="671" height="681" alt="image" src="https://github.com/user-attachments/assets/b32a4e26-2800-4372-97b0-c20df727d209" />

From graph , Vm = 0.81V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.585um l=130nm
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

## plot : transient analysis

<img width="872" height="659" alt="image" src="https://github.com/user-attachments/assets/3ff50515-31ae-488b-aa53-235e7c92d638" />

Rise time = 0.125ns

Fall time = 0.187ns

---

# Case 4 : Wn=0.195um,Wp=0.78um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 4 ( $\frac{Wp}{Lp}$ )

## Netlist for VTC: 
```
.include cmos_65nm.txt

M1 out in vdd vdd pmos w=0.78um l=0.13um
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

## Plot : VTC Curve 

<img width="671" height="730" alt="image" src="https://github.com/user-attachments/assets/eb10ca36-8da4-4c98-841c-2027d6dac80d" />

From graph , Vm = 0.87V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.78um l=130nm
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

## plot : transient analysis


Rise time = 0.125ns

Fall time = 0.187ns

---
