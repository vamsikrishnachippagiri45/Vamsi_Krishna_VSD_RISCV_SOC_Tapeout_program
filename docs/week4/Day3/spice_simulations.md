# Static Behaviour Evalution - Switching Threshold(Vm)

## case 1 : Wn = Wp = 0.375u , Ln = Lp = 0.25u ,  $\frac{Wn}{Ln}$ = $\frac{Wp}{Lp}$ = 1

### Netlist : 
```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.375 l=0.25
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.375 l=0.25

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

### Plot : VTC Curve 


