
#  INSTALLING OPENROAD USING CMAKE 

---

## STEP 1: Clone the OpenROAD Flow Repository

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```

###  Explanation:

* `git clone --recursive` → downloads the **OpenROAD-flow-scripts** repo **and its submodules** (OpenROAD, Yosys, etc.).
* `cd OpenROAD-flow-scripts` → moves into the project directory where all scripts and setup files are located.

Without `--recursive`, submodules (like Yosys, OpenROAD, etc.) won’t be downloaded, causing build errors later.

---

## STEP 2: Install Dependencies Automatically

```bash
sudo ./setup.sh
```

### Explanation:

* Runs the **setup script** that installs all the required dependencies.
* These include:

  * **Build tools**: `gcc`, `g++`, `cmake`, `make`
  * **EDA dependencies**: `boost`, `swig`, `libreadline-dev`, etc.
  * **Python packages** for flow automation.
* It also ensures that **OpenROAD, Yosys, OpenSTA, KLayout**, and other tools can be built properly.

---

## STEP 3: Build OpenROAD Locally

```bash
./build_openroad.sh --local
```

###  Explanation:

* This script **compiles and builds** the OpenROAD tool and other components using **CMake**.
* The `--local` flag means:
  → build everything **inside your local system**, not in Docker or any container.
* After this finishes, it creates all compiled binaries inside subfolders like:

  ```
  tools/OpenROAD/build/
  ```

>  It automatically logs everything into a file:
>
> ```
> build_openroad.log
> ```
>
> If something fails, open this log file to see the exact error.

---

##  STEP 4: Load Environment Variables

```bash
source ./env.sh
```

###  Explanation:

* This command **sets up your PATH and environment variables** temporarily in the current terminal.
* It adds all built tool binaries (OpenROAD, Yosys, OpenSTA, etc.) to your `$PATH`.
* This allows you to run tools like `openroad`, `yosys`, and `opensta` directly from the terminal.
* You need to run this command **every time you open a new terminal** (or add it to `.bashrc` for convenience).

---

##  STEP 5: Verify Installation

```bash
yosys -help
openroad -help
```

###  Explanation:

* These commands test if the binaries were built and added to your `$PATH` correctly.
* If both commands show help information — OpenROAD and Yosys are successfully installed!

---

# STEP 6: Running the Default Example (GCD Design)

The OpenROAD-flow-scripts repo already includes a **ready-made example design**:
`flow/designs/nangate45/gcd`

We’ll use this to test the full RTL → GDSII flow.

---

### 1️⃣ Go to the flow directory:

```bash
cd flow
```

### 2️⃣ Run the default GCD flow:

```bash
make
```

###  What Happens Internally

When you type `make`, OpenROAD runs several stages in order:

| Stage                          | Tool                         | Description                               |
| ------------------------------ | ---------------------------- | ----------------------------------------- |
| 1️⃣ Synthesis                  | **Yosys**                    | Converts Verilog RTL → gate-level netlist |
| 2️⃣ Floorplan                  | **OpenROAD**                 | Defines chip area, macros, I/O pins       |
| 3️⃣ Placement                  | **OpenROAD**                 | Places standard cells optimally           |
| 4️⃣ CTS (Clock Tree Synthesis) | **OpenROAD**                 | Builds clock tree network                 |
| 5️⃣ Routing                    | **TritonRoute**              | Connects signals and power lines          |
| 6️⃣ STA                        | **OpenSTA**                  | Checks timing (setup/hold)                |
| 7️⃣ Signoff                    | **OpenROAD + Magic/KLayout** | DRC/LVS, final GDS export                 |

All generated results go here:

```
flow/results/nangate45/gcd/
```

You’ll see subfolders like:

```
1_synth/
2_floorplan/
3_place/
4_cts/
5_route/
6_final/
```

And final outputs:

```
results/nangate45/gcd/6_final/gcd.gds
results/nangate45/gcd/6_final/gcd.def
results/nangate45/gcd/6_final/gcd.sdc
results/nangate45/gcd/6_final/gcd.v
```

---

###  View Final Layout (Optional GUI Mode)

After the flow finishes:

```bash
make gui_final
```

This opens **OpenROAD GUI** showing:

* Standard cell placement
* Routing layers
* Pins and power grid

It’s an interactive layout viewer.

---

# STEP 7: Running a **New Design**

Suppose you have your own Verilog design (for example, `adder.v`).
You can easily run the same flow for it.

---

### 🧱 1️⃣ Create a Folder for Your Design

Go to:

```bash
cd flow/designs/nangate45/
mkdir my_design
cd my_design
```

---

### 🧾 2️⃣ Add Your Verilog File(s)

Copy your RTL files into this folder:

```bash
cp ~/path/to/adder.v .
```

---

### ⚙️ 3️⃣ Create a Configuration File (`config.mk`)

This file tells OpenROAD what your design is called and which file is top-level.

Create `config.mk` inside your folder:

```bash
nano config.mk
```

Add this content:

```makefile
export DESIGN_NAME = adder
export DESIGN_NICKNAME = adder
export VERILOG_FILES = ./adder.v

# Clock configuration (modify if your design has a clock)
export CLOCK_PORT = clk
export CLOCK_PERIOD = 10.0

# Optional (you can keep defaults)
export SYNTH_READ_LIB = ./../../lib/nangate45/NangateOpenCellLibrary_typical.lib
export DESIGN_POWER_PORT = VDD
export DESIGN_GROUND_PORT = VSS
```

> 💡 Replace `adder`, `clk`, and file paths with your actual names if different.

---

### 🔧 4️⃣ Run the Flow for Your Design

From the `flow/` directory:

```bash
cd ../../
make DESIGN_CONFIG=designs/nangate45/my_design/config.mk
```

This command tells OpenROAD:

> “Run the flow using the settings defined in my config.mk.”

It will automatically:

* Read your RTL
* Synthesize
* Place & route
* Generate `GDSII`, `DEF`, `LEF`, and timing reports

---

### 📂 5️⃣ Check Results

After successful completion:

```
flow/results/nangate45/my_design/
```

You’ll find:

* `adder.gds` → Final layout (viewable in GUI)
* `adder.def` → Physical design file
* `adder.v` → Post-synthesis netlist
* `adder.sdc` → Timing constraints
* `reports/` → Timing, power, and area reports

---

### 👁️ 6️⃣ View Layout in GUI

```bash
make DESIGN_CONFIG=designs/nangate45/my_design/config.mk gui_final
```

This opens the GUI for your custom design.

---

# 🧾 SUMMARY TABLE

| Task                    | Command                                                              | Description                    |
| ----------------------- | -------------------------------------------------------------------- | ------------------------------ |
| Run default example     | `cd flow && make`                                                    | Runs built-in GCD design       |
| View layout             | `make gui_final`                                                     | Opens final GDS layout         |
| Add new design          | Create `flow/designs/nangate45/my_design/`                           | Custom design folder           |
| Add RTL                 | Copy `*.v` files                                                     | Place your design files        |
| Create config           | `nano config.mk`                                                     | Define top module, clock, etc. |
| Run new design          | `make DESIGN_CONFIG=designs/nangate45/my_design/config.mk`           | Executes full flow             |
| GUI view for new design | `make DESIGN_CONFIG=designs/nangate45/my_design/config.mk gui_final` | Shows layout                   |

---

Would you like me to give you a **ready-made example folder and config.mk** (for, say, a 4-bit adder or counter), so you can copy it into `designs/nangate45/` and run it directly?


