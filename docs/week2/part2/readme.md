

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

### (i) Convert TLV File to verilog/system verilog

The repository includes a file rvmyth.tlv (Transaction-Level Verilog).
TLV is not directly supported by iverilog, hence it cannot be compiled as-is.
To use the TLV description, one needs the SandPiper tool from Redwood EDA, which converts .tlv files into synthesizable SystemVerilog (.sv) files. This is the Sandpiper Method of simulation.

### (ii) Stub Method (Used Here):

A stub is a simplified placeholder module that mimics the interface (inputs/outputs) of a real module but does not implement its full functionality.
Using the stub allowed me to compile and run the SoC testbench with iverilog, ensuring that the integration and connectivity of the SoC modules could be checked.
This method bypasses TLV translation and is mainly used for quick verification of the SoC flow.

The Stub used for pre-synthesis simulation is :
```
module rvmyth_stub(
   output real OUT,
    input CLK,
    input reset
);
    // 10-bit digital counter
    reg [9:0] counter;   // goes from 0 to 1023 (3FF)
    reg dir;             // direction: 0 = up, 1 = down

    always @(posedge CLK or posedge reset) begin
        if (reset) begin
            counter <= 10'd0;
            dir <= 1'b0; // start by counting up
        end else begin
            if (!dir) begin
                if (counter == 10'd1023) begin
                    dir <= 1'b1;          // switch to down
                    counter <= counter - 1;
                end else begin
                    counter <= counter + 1;
                end
            end else begin
                if (counter == 10'd0) begin
                    dir <= 1'b0;          // switch to up
                    counter <= counter + 1;
                end else begin
                    counter <= counter - 1;
                end
            end
        end
    end

    // Convert digital counter to real (for DAC output visualization)
    always @(*) begin
        OUT = counter * 1.0;   // scale if needed, e.g., * (3.3/1023.0) for volts
    end

endmodule
```

Use case: Learning SoC integration, debugging processor-peripheral connections, verifying SoC design flow.


## Presynthsis simulation 


<img width="1376" height="365" alt="image" src="https://github.com/user-attachments/assets/1f0ae13c-4443-453b-a411-c794383a9b2e" />

The terminal output shows three critical steps executed from the main project directory (~/VLSI/VSDBabySoC):

#### 1. Compilation (iverilog)

The first command compiles all necessary Verilog source files into a single simulation executable.

Command:
```
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
```
Purpose: This tells the compiler to take all design files (testbench.v and any files inside the specified include paths, which would contain the SoC's components like the PLL, DAC, and RVMYTH) and create an executable output file (-o). The macro -DPRE_SYNTH_SIM is often used to activate behavioral models (stubs) or simulation-specific logic within the Verilog files.
This creates an executable file pre_synth_sim.out.

#### 2. Simulation Execution (./pre_synth_sim.out)

The second command executes the compiled simulation file using the vvp runtime engine (which is implicitly called when executing the .out file directly).
```
./output/pre_synth_sim/pre_synth_sim.out
```
 Output: The simulation log confirms:
        A Value Change Dump (.vcd) file named pre_synth_sim.vcd was opened for waveform output.
        The testbench execution finished ($finish called) at time 169998000 (picoseconds).
This shows Generation of the pre_synth_sim.vcd file, which contains the entire signal history (the Simulation Log).

#### 3. Waveform Analysis (gtkwave)

The final command loads the simulation results into the waveform viewer.
Command: 
```
gtkwave pre_synth_sim.vcd
```
Output: GTKWave starts and reports the time boundaries of the simulation run.
The user can now graphically analyze the signals (clocking, reset, and dataflow) to verify the BabySoC's functional correctness.

### Observations in GTKWave

#### Core (RVMYTH Stub) verification

<img width="1520" height="630" alt="Screenshot from 2025-10-04 05-22-21" src="https://github.com/user-attachments/assets/1ad53016-1d59-49ff-8238-13f46f13ea31" />

RVMYTH Core Reset and Initialization:
Signals : 

reset: The signal is initially high (asserted) or transitioning from high to low.

CLK: The clock signal is stable and continuously pulsing (provided by the PLL model).

counter[9:0]: This is the internal data output of the stub. It is shown initially as XXX (unknown) or 000 during reset.

Data Transition: The moment the reset signal is deasserted (goes low), the counter immediately transitions to a known value (000) and starts changing sequentially.

Verification: This confirms the reset operation is successful. The core (stub) moves from an unknown or reset state to a functional state (000) exactly when the reset signal is removed.

<img width="1520" height="636" alt="Screenshot from 2025-10-04 05-23-25" src="https://github.com/user-attachments/assets/15f81ae3-0356-4a70-bc82-59644741c45f" />


#### Clocking verification 
This waveform snippet confirms the clock source operation within the BabySoC. 

<img width="1520" height="620" alt="image" src="https://github.com/user-attachments/assets/3b498d0a-46d4-4821-809b-4bdbe9c76b0f" />

This plot confirms that the clock signal, essential for synchronous operation, is stable and correctly available to the digital logic from the start of the simulation. 
It validates the clocking interface, ensuring the RVMYTH core (stubbed) receives the required synchronization signal to begin its operations.

### DAC verification

<img width="1520" height="633" alt="image" src="https://github.com/user-attachments/assets/625c5376-7f6d-4e59-b467-983a2935bbd5" />

This close-up view shows the functional steps of the DAC in response to the processor's output:
The D[9:0] signal, which is the 10-bit digital input from the processor (likely derived from a register write), changes its value incrementally (e.g., 000→001→002...). This confirms the processor is executing its test program and successfully driving the DAC interface.

The OUT signal, which is modeled as a real (analog) data type, changes immediately after the digital input D[9:0] changes.
Verification: This confirms the digital interface is working and the behavioral model of the DAC is correctly translating the incremental digital input values into corresponding real (analog) output voltage levels.

The time delay you observe in the first few nanoseconds (ns) of the simulation, before the signals settle into a stable, continuous pattern, is the Reset and Initialization Phase. 

<img width="1520" height="633" alt="image" src="https://github.com/user-attachments/assets/1c1f0cfd-b373-4e46-a1b3-7b88d458adaa" />

This wider view over a longer simulation time shows the cumulative effect of the processor's program:
The D[9:0] input is shown transitioning across a range of values (the thick green band).
The OUT signal displays a smooth, continuous analog waveform.

Verification: This validates the overall system-level functionality where the processor's sequence of digital data writes results in the desired analog output pattern. It confirms that the dataflow path from the RVMYTH core, through the SoC wrapper, and into the DAC interface is fully functional.


