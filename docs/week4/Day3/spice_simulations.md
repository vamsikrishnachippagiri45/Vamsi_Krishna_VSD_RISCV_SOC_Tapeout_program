# Static Behaviour Evalution - Switching Threshold(Vm)

Spice simulation is done using 130nm technology model file. This model file is in Day3 folder.

# Case 1 : Wn = Wp = 0.195um , Ln = Lp = 65nm ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ 

## Netlist for VTC: 
```
.include cmos_130nm.txt

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

<img width="551" height="620" alt="image" src="https://github.com/user-attachments/assets/8d21fb4f-8d89-4c85-9d7f-866fd982f448" />


From graph , Vm = 0.7V 


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
.include cmos_130nm.txt

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

<img width="585" height="640" alt="image" src="https://github.com/user-attachments/assets/dfa03bb9-5077-4c25-8d35-978faa90bd6b" />

From graph , Vm = 0.86V 


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
.include cmos_130nm.txt

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

<img width="627" height="684" alt="image" src="https://github.com/user-attachments/assets/6d765c54-6057-4ebc-aea5-4a5aa14304c5" />


From graph , Vm = 0.95V 


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
.include cmos_130nm.txt

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

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/7cf8623e-5790-4434-9150-91e77e8a90c8" />


From graph , Vm = 1.01V 


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

<img width="833" height="612" alt="image" src="https://github.com/user-attachments/assets/98d89694-4fb2-429e-9128-21887e41d454" />

Rise time = 0.097ns

Fall time = 0.194ns

---

# Case 5 : Wn=0.195um,Wp=0.975um,Ln=Lp=65nm, ( $\frac{Wn}{Ln}$ ) = 5 ( $\frac{Wp}{Lp}$ )

## Netlist for VTC: 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.975um l=0.13um
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

<img width="632" height="684" alt="image" src="https://github.com/user-attachments/assets/2933e827-c96f-45e9-8785-cebbd57144eb" />

From graph , Vm = 1.06V 


## Netlist for transient analysis

```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.975um l=130nm
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

<img width="835" height="635" alt="image" src="https://github.com/user-attachments/assets/9f8d8827-cfe0-43e6-9b5d-2f1fc8bb6ef9" />

Rise time = 0.090ns

Fall time = 0.188ns

---


