# CMOS Inverter Robustness – (iii) Power Supply Scaling


## Spice simulation
### Netlist 
```
.include cmos_130nm.txt

M1 out in vdd vdd pmos w=0.495um l=130nm
M2 out in 0 0 nmos w=0.195um l=130nm

Cload out 0 0.1fF

Vdd vdd 0 2V
Vin in 0 2V

.control 
let powerSupply = 2 
alter Vdd = powerSupply

    let voltageSupplyVariation = 0
    dowhile voltageSupplyVariation < 4
        dc Vin 0 2 0.01
        let powerSupply = powerSupply - 0.5
        alter Vdd = powerSupply
        let voltageSupplyVariation = voltageSupplyVariation + 1 
    end
    
    plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in  xlabel "Input Voltage[V]" ylabel "Output Voltage[V]" title "Inverter DC Characteristics as a function of supply voltage"
    * quit 
.endc 
.end
```

<img width="746" height="684" alt="image" src="https://github.com/user-attachments/assets/044f7960-1374-46a8-b70d-0fb6c67a96d7" />

for vdd = 2v , gain = 2.9
for vdd = 1.5v , gain = 3.6
for vdd = 1v , gain = 5.67
for vdd = 0.5v , gain = 5.81
