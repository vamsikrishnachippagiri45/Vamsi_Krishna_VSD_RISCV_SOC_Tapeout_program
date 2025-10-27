
#  INSTALLING OPENROAD USING CMAKE 

---

## STEP 1: Clone the OpenROAD Flow Repository

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```

<img width="1686" height="365" alt="image" src="https://github.com/user-attachments/assets/b3bad8b1-4692-439e-9c7c-25b7058d8e81" />



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


<img width="1140" height="386" alt="image" src="https://github.com/user-attachments/assets/86532ad3-91ae-42f4-a83e-49f53dd7a38e" />


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


<img width="1573" height="378" alt="image" src="https://github.com/user-attachments/assets/279e5b88-ed82-4fff-8a55-cb07d7eee909" />


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

<img width="1854" height="890" alt="image" src="https://github.com/user-attachments/assets/16d1ff5a-c307-4469-8d12-cdd9b24fff58" />


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

<img width="1706" height="497" alt="image" src="https://github.com/user-attachments/assets/96f2ffc1-f756-42aa-8b98-d1db211e9739" />


<img width="1854" height="890" alt="image" src="https://github.com/user-attachments/assets/4c23abb3-e7c0-492e-bdb8-5a77702379fc" />


This opens **OpenROAD GUI** showing:

* Standard cell placement
* Routing layers
* Pins and power grid

It’s an interactive layout viewer.

---
