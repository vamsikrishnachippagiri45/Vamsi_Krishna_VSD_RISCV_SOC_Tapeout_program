

# Functional simulation of VSDBabySoC (Pre-synthesis simulation)

Functional simulation, or pre-synthesis simulation, is the first and most critical step in verifying any digital design. Its objective is to validate the design's logic and behavior against the specification, irrespective of timing delays or physical implementation.

For the VSDBabySoC, the goal is to confirm the high-level operation and data integrity among the main IP blocks: the RVMYTH processor, the PLL , and the DAC.

## Clone the repository 

You will execute the following command in your terminal. Since you are working in a specific location (/home/vamsi/VLSI/), you should navigate there first.
Use the git clone command followed by the repository URL provided.
```
cd /home/vamsi/VLSI/
git clone https://github.com/manili/VSDBabySoC
```


<img width="1006" height="343" alt="image" src="https://github.com/user-attachments/assets/e42d9c45-e720-47d4-8c16-9dd704e53890" />

The terminal output confirms the successful location of the necessary source files within the project hierarchy.
Module Directory (src/module/): Contains all the core IP component => Peripherals (Stubs): avsddac.v, avsdpll.v  , SoC Wrapper: vsdbabysoc.v &  RVMYTH Core Source: The untranslatable file, rvmyth.tlv.


The RVMYTH core's primary source file (rvmyth.tlv) is unreadable by the Icarus Verilog simulator. The following two methods allow the processor to be included in the functional simulation:

### Method 1: Translation (Full Path) 

This approach utilizes an external tool to convert the core to a standard HDL format, enabling simulation of the full RVMYTH logic.

Source Files Used: rvmyth_verilog.sv and rvmyth_verilog_gen.sv.

Pros:Full Verification: Simulates the complete RVMYTH core logic as intended by the design. High Fidelity: Provides higher confidence in the processor's functional correctness before synthesis.
Cons: External Dependency: Requires the use of the external sandpiper−saas tool (and a Python virtual environment) to generate the necessary .sv files prior to compilation.

 ### Method 2: Stubbing (Simple Path)

This approach uses a pre-written behavioral model to bypass the core's complexity, allowing immediate system-level testing.

Source File Used: rvmyth_stub.v.

Pros: Simplest Execution: Requires no external tools or file conversion steps. Fast Setup: Allows immediate integration and verification of the system's wrapper and peripherals (PLL/DAC).

Cons:
        Limited Verification: Only verifies the interface (I/O signals and ports). The actual processor logic is replaced by a simplified model (e.g., a counter or hard-coded sequence), meaning the core itself is not functionally tested.
