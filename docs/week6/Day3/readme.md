
# **16-mask CMOS Fabrication process**
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
