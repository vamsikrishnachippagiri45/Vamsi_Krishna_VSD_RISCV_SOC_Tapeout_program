
# **16-Mask CMOS Fabrication process**
---

## Steps of 16-Mask CMOS Fabrication

### 1. Substrate Selection

* **Material:** Fabrication begins with a **P-substrate** (P-type silicon wafer).
* **Characteristics:** This substrate is typically lightly doped, having **high resistivity** (e.g., $5$ to $50$ ohms), and a low doping level ($\sim 10^{15} \text{ cm}^{-3}$). The common crystal orientation is $(100)$.


### 2. Creating Active Regions (Isolation)

This section defines the regions where transistors will be built (**Active Regions**) and isolates them using thick oxide (**Field Oxide**).

* **Initial Layers:** A thin layer of $\text{SiO}_2$ (silicon dioxide, $\sim 40\text{nm}$) and a layer of $\text{Si}_3\text{N}_4$ (silicon nitride, $\sim 80\text{nm}$) are deposited over the entire wafer. A layer of **photoresist** ($\sim 1\mu\text{m}$) is then applied.
* **Mask 1 (Active Area Definition):**
    * **Photolithography:** The wafer is exposed to **UV light** through **Mask 1**, which patterns the photoresist, defining the active regions.
    * **Etching:** The exposed photoresist is removed, and the underlying $\text{Si}3\text{N}4$ layer is etched in these open areas.
    * **Field Oxide Growth (LOCOS):** The wafer is subjected to a high-temperature oxidation process. The $\text{Si}3\text{N}4$ acts as an **oxidation mask**, preventing oxide growth beneath it. Oxide only grows in the exposed areas, forming the thick **Field Oxide** (isolation layer). This technique is called **LOCOS** (**LO**cal **O**xidation of **S**ilicon).
    * **"Bird's Beak" Effect:** LOCOS is characterized by a "bird's beak" shape at the boundary of the field oxide due to lateral oxidation under the $\text{Si}3\text{N}4$ mask, which consumes some active area.
    * **Stripping:** The $\text{Si}3\text{N}4$ layer is stripped using a chemical like hot phosphoric acid, leaving the isolated active areas framed by the Field Oxide.


### 3. N-Well and P-Well Formation

Wells are the regions required to hold transistors of the opposite type (NMOS in P-well, PMOS in N-well).

* **P-Well Formation (Optional/Implicit):** Since the starting material is a P-substrate, a separate P-well formation step may not be required for the entire area, but often a separate deep P-well is needed for isolation.
* **N-Well Formation:**
    * **Masking:** A new photoresist layer (**Mask 2**) is used to protect the P-well regions while leaving the N-well areas exposed.
    * **Ion Implantation:** **N-type dopants** (e.g., **Phosphorous**) are implanted into the exposed active regions to create the N-well.
    * **P-Well Implant:** A separate mask (**Mask 3**) is used, and **P-type dopants** (e.g., **Boron**) are implanted to form/adjust the P-well regions, often compensating for previous implants.
    * **Drive-in Diffusion:** The wafer is placed in a **high-temperature furnace** for a long time. This step allows the implanted dopants to **diffuse** deeper and laterally into the silicon, fully forming the required **N-well** and **P-well** structures.


### 4. Gate Oxide and Gate Formation

This step creates the most critical part of the transistor: the gate and the insulating oxide beneath it.

* **Pre-gate Oxide Stripping:** The original thin oxide layer present over the active areas (which has been exposed to high-temperature well formation) has a degraded quality. This original oxide is **etched/stripped** using a solution like **dilute hydrofluoric (HF) acid**.
* **Gate Oxide Regrowth:** A new, thin, **high-quality oxide** ($\sim 10\text{nm}$ thick) is **re-grown**. This oxide will serve as the final **gate insulator**, and its precise thickness is crucial for controlling the transistor's threshold voltage ($\text{Vth}$) and performance.
* **Gate Material Deposition:** A layer of **polysilicon** is deposited over the entire wafer.
* **Mask 6 (Gate Definition):** A new photoresist layer (**Mask 6**) is used to pattern the polysilicon, leaving material only where the **gate** of each transistor is desired.

### 5. Lightly Doped Drain (LDD) Formation

The LDD structure is implemented in advanced CMOS processes to mitigate undesirable high-field effects that become severe as transistors are scaled down (Short Channel Effects).

#### 1. Reasons for LDD

LDD structures are necessary to combat two primary issues in small transistors:

* **Hot Electron Effect (HCE):** In short-channel devices, the **electric field ($E = V/d$)** near the drain is extremely high. This high field accelerates electrons (or holes) to very high energies (**hot carriers**). These high-energy carriers can collide with silicon bonds and become trapped in the **Gate Oxide ($\text{SiO}_2$)**, breaking the $3.2\text{eV}$ barrier between the silicon and $\text{SiO}_2$ conduction bands. Trapped charges shift the transistor's **Threshold Voltage ($\text{Vth}$)**, leading to degradation and eventual failure.
* **Short Channel Effect:** As the channel length decreases, the depletion regions of the source and drain start to interact, leading to issues like **punch-through** and loss of gate control.

The LDD structure lowers the electric field peak near the drain, reducing the energy of the carriers and minimizing HCE.

#### 2. LDD Implantation and Spacer Formation

The LDD structure is created through a self-aligned process involving a light implant followed by the creation of a **sidewall spacer**.

1.  **Light Implant:**
    * A **light dose** of N-type dopants (for NMOS, typically **Phosphorous** or **Arsenic**) and P-type dopants (for PMOS, typically **Boron**) is implanted into the exposed Source/Drain regions.
    * This implant is performed **unmasked** over the whole wafer, but the existing polysilicon gate acts as a **mask**, defining the inner edge of the LDD region.
    * This creates the $\mathbf{N}^{-}$ (lightly doped N) and $\mathbf{P}^{-}$ (lightly doped P) regions directly adjacent to the channel under the gate edge. 
2.  **Sidewall Spacer Formation:**
    * A layer of material (usually **silicon dioxide** or **nitride**) is deposited over the entire wafer.
    * A **Plasma Anisotropic Etching** step is performed. Since the etching is anisotropic (etching only vertically), the material is removed from the horizontal surfaces but remains along the vertical sidewalls of the polysilicon gate, forming **sidewall spacers**. 
The spacers will now be used as the **mask** for the next step: the heavy Source/Drain implant. The spacer shifts the final, heavy Source/Drain junction away from the sensitive gate edge, defining the final LDD structure.

These notes summarize the final stages of the **16-Mask CMOS Fabrication Process**, detailing the formation of the heavily doped Source/Drain regions and the multi-level metal interconnects.

### 6. Source and Drain Formation

This step creates the highly conductive Source ($S$) and Drain ($D$) regions, completing the transistor structure.

* **Initial Condition:** The structure already has the polysilicon gate with the $\text{SiO}_2$ $\text{Si}_3\text{N}_4$ **sidewall spacers** and the lightly doped ($\text{N}^{-}$, $\text{P}^{-}$) LDD regions.
* **Thin Screen Oxide:** A thin $\text{SiO}_2$ layer is often grown or deposited before the implant to **prevent channeling** (where ions travel too deep) during the high-energy implant step.
* **Heavy Implant:** A **heavy dose** of N-type dopants (for NMOS $\text{N}^+$) and P-type dopants (for PMOS $\text{P}^+$) is implanted.
    * The **sidewall spacers** act as a **mask**, defining the final outer edge of the heavily doped $\text{S}/\text{D}$ regions, placing them away from the sensitive channel area.
* **High Temperature Annealing:** The wafer is placed in a **high-temperature furnace** for a process called **annealing**. This step achieves two goals:
    * **Electrical Activation:** It repairs the crystal damage caused by the ion implantation.
    * **Dopant Activation:** It moves the implanted ions into electrically active substitutional sites within the silicon lattice, completing the $\text{N}^+$ and $\text{P}^+$ Source/Drain regions.

### 7. Local Contacts and Interconnects (Local Wiring)

This phase prepares the transistor terminals for connection using the first layer of metal (M1).

1.  **Oxide Etching:** The thin $\text{SiO}_2$ layer covering the $\text{S}/\text{D}$ and gate terminals is etched away using an **HF solution** to expose the silicon and polysilicon surfaces for contact formation.
2.  **Titanium (Ti) Deposition:** A layer of **Titanium** is deposited over the wafer surface, often using a process called **sputtering** (bombarding a target with Argon ions to deposit the material).
3.  **Salicide Formation:** The wafer is heated ($\approx 650-700^{\circ}\text{C}$ in $\text{N}_2$ ambient). The deposited titanium reacts with the exposed silicon/polysilicon to form **Titanium Silicide ($\text{TiSi}_2$)**—a highly **low-resistance** compound. The titanium that did not react with silicon (i.e., over the oxide) is etched away (often using **RCA cleaning**).
4.  **Local Metal Deposition (M1):** The $\text{TiSi}_2$ and a **TiN** (Titanium Nitride) layer are often used as a local interconnect layer for immediate connections between adjacent transistors (local communication) before the main global metal layers are added.

### 8. Higher Level Metal Formation (Global Wiring)

This final phase builds the global interconnect structure, forming the complete wiring network (power, clock, and data).

1.  **Inter-Layer Dielectric (ILD) Deposition:** A thick insulating layer, typically **$\text{SiO}_2$ doped with phosphorous or boron** (known as **Phosphosilicate Glass (PSG)** or **Borophosphosilicate Glass (BPSG)**), is deposited over the entire structure. This dielectric electrically separates the metal layers.
2.  **Contact/Via Etching:** Using photolithography and masks, **vertical holes** are etched through the oxide to reach the $\text{TiSi}_2$ and polysilicon terminals below.
3.  **Tungsten (W) Plug Formation:** The holes are filled with a highly conductive metal, usually **Tungsten (W)**, which forms the **contacts** (connecting M1 to the source/drain/gate) and **vias** (connecting metal layers to each other).
4.  **Metal Interconnect Deposition:** Layers of metal (e.g., Aluminum or Copper) are deposited, patterned, and etched to form the horizontal interconnect lines ($\text{M}1, \text{M}2, \text{M}3$, etc.).
5.  **Final Structure:** This iterative process (dielectric deposition, etching vias, filling vias, depositing metal, patterning metal) continues for all 16 masks, resulting in the final, complex 3D structure that connects the $S, G, D$ terminals of all transistors into a functional chip.



---
