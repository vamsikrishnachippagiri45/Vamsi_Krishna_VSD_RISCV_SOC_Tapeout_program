

## Standard Cell Design

This procedure involved cloning an external standard cell repository, copying the necessary technology file, and then loading a cell's layout for visual inspection using the Magic tool.

### I. Setup Commands and Directory Navigation

The following commands were executed in the terminal to set up the environment:

1.  **Navigate to OpenLANE Working Directory:**

    ```bash
    cd Desktop/work/tools/openlane_working_dir/openlane
    ```

2.  **Clone the Standard Cell Design Repository:**
    The `vsdstddcelldesign` repository, containing the layout of the standard cell, was cloned from GitHub.

    ```bash
    git clone https://github.com/nicksonm-jose/vsdstddcelldesign.git
    ```
    
<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/8e375f48-9d42-4358-b593-0de51aa52ec4" />


3.  **Copy Technology File:**
    The **PDK technology file** (`sky130A.tech`) is essential for Magic to understand the design rules and layer definitions for the specific process (SkyWater 130nm). This file was copied from the PDK directory to the new design directory.

    ```bash
    # (Navigated to pdks/sky130A/libs.tech/magic first)
   cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
    ```

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/253234b8-f907-46c4-81cc-9c0e8dca8d0f" />

4.  **Change Directory to Design:**

    ```bash
    cd vsdstddcelldesign
    ```

5.  **List Contents:**
    The file listing (`ls -ltr`) confirmed the cloned files and the copied `sky130A.tech` file were present. The cell layout file, named `sky130_inv.mag`, was identified.

### II. Magic Tool Invocation and Visualization

The Magic VLSI layout editor was invoked to open and display the standard cell layout.

1.  **Magic Command:**
    The command tells Magic to use the specified technology file (`-T sky130A.tech`) and load the cell layout file (`sky130_inv.mag`).
    ```bash
    magic -T sky130A.tech sky130_inv.mag &
    ```
2.  **Visualization:**
    The **physical layout** of the inverter cell (`sky130_inv`) was displayed. The layout shows various colored polygons representing the different fabrication layers, such as:
      * **$\text{V}{\text{PWR}}$ (Power) and $\text{V}{\text{GND}}$ (Ground)** supply rails (blue/purple hatched areas).
      * **Gate (Polysilicon)** and **Diffusion (Active Area)** to form the transistors.
      * **Contacts** ($\text{X}$ marks) for connecting metal layers to the transistors.

<img width="1920" height="923" alt="image" src="https://github.com/user-attachments/assets/5cd84c90-c6a4-4491-9748-f87e9bf9ab88" />
