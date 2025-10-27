
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


