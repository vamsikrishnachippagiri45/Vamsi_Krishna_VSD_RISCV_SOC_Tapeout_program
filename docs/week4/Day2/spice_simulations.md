# Spice simulations 

## 1) Long channel MOSFET ( W=1.8um , L=1.2um , W/L = 1.5)

#### Spice Netlist for Id Vs Vds:

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
#### Plot : Id Vs Vds 

<img width="1243" height="781" alt="image" src="https://github.com/user-attachments/assets/2ccbe7eb-9f34-4774-9d26-728567bb45b8" />

#### Spice Netlist for Id Vs Vgs:

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

#### Plot : Id Vs Vgs  at Vds = 1.8V

<img width="1147" height="780" alt="image" src="https://github.com/user-attachments/assets/1985b759-ee01-455a-8c85-2b12d2ee4de8" />



