# Floorplanning 


##  OpenLANE Configuration Hierarchy

OpenLANE uses a layered approach to configuration, where specific design settings override more general, default settings. This hierarchy ensures that every design step has the precise geometric and electrical parameters it needs.

1.  **OpenLANE Defaults (Base):**
    * Found in the main `$OPENLANE_ROOT/configuration` directory (e.g., `floorplan.tcl`, `config.tcl`). These files provide the foundational, generic settings for the entire flow.
2.  **Design Defaults (PDK-Specific):**
    * Found within the `$OPENLANE_ROOT/designs/<design_name>/` folder (e.g., `sky130_fd_sc_hd_config.tcl`).
    * These files override the base settings with values specifically tuned for the chosen **PDK** (e.g., `sky130A` and its standard cell library, `sky130_fd_sc_hd`).
3.  **User Configuration (Highest Priority):**
    * The primary file you modify to customize a specific design run (e.g., `config.tcl` inside the design folder).
    * **These settings override all others** and are used to fine-tune the design goals (speed vs. area) and physical parameters for the current project.


| Variable | Description | Default/Example Value | Design Impact |
| :--- | :--- | :--- | :--- |
| `FP_CORE_UTIL` | **Core Utilization Percentage.** The ratio of the total area of the standard cells (netlist) to the total available core area. | `50` (or `50%`) | Controls the **density** of the design. A lower number leaves more space for routing and is safer but wastes area. |
| `FP_ASPECT_RATIO` | The ratio of the core's height to its width ($\text{Height} / \text{Width}$). | `1` (for a square core) | Controls the **shape** of the core. Typically set to 1 for simplicity unless a specific chip shape is required. |
| `FP_IO_VLENGTH` | The vertical length of the I/O pins (in microns). | `4` | Controls the **size** of the pads on the chip edges. |
| `FP_IO_HLENGTH` | The horizontal length of the I/O pins (in microns). | `4` | Controls the **size** of the pads on the chip edges. |
| `FP_HORIZONTAL_HALO` | The margin (empty space) set between the core logic and the horizontal edges of the die. | N/A | Reserves space for Decap cells and power integrity structures. |
| `FP_PNR_AUTO_ADJUST` | Determines if the tool can automatically adjust the power grid spacing/size to fit the core area. | `1` (Enabled) | Allows the tool to automatically fill the core with power rings/straps. |

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/fd8f9d1d-4470-4ecd-9be9-1a5fe2214d90" />

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/74400b4d-7182-45b5-beaa-3d6963c976e6" />

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/0a6f2f64-3929-4d42-90c0-bc3a9897b6c9" />




---

file:///home/vsduser/Pictures/Screenshot%20from%202025-10-30%2021-32-05.png<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/e88f8a60-2bc8-4580-bc85-96b3b461b266" />


file:///home/vsduser/Pictures/Screenshot%20from%202025-10-30%2021-32-52.png<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/9519608f-1ed8-46a6-95b7-9c166d409df8" />

file:///home/vsduser/Pictures/Screenshot%20from%202025-10-30%2021-38-46.png<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/545a7026-35d8-44c2-9ce4-08c11f55be97" />

file:///home/vsduser/Pictures/Screenshot%20from%202025-10-30%2021-48-55.png<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/2da6a400-231f-465b-94fa-c47a522a2779" />

