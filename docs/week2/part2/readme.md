

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


