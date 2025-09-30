# System on Chip (SoC) Fundamentals

## What is an SoC 

Think of an old desktop computer. You have the CPU plugged into a big motherboard, which has cables connecting it to the graphics card, the memory chips, the USB ports, and so on.

A System-on-Chip (SoC) is like miniaturizing that entire desktop system onto a single, tiny fingernail-sized piece of silicon. It’s an electronic "city" where all the essential services—the brain, the highways, the post office, and the power grid—are built into one compact area.

A System-on-Chip (SoC) is an electronic circuit that integrates almost all the components of a complete electronic system—like a phone or a simple computer—onto a single, small silicon chip. Instead of a large circuit board (a motherboard) connecting many separate chips for the CPU, memory control, and communication, the SoC puts all that functionality into one tiny package.

## Why SoCs are Essential

The driving force behind SoC design is efficiency and size. Because all the internal parts are physically right next to each other:

Power is Saved: Data signals don't have far to travel, requiring much less energy. This is crucial for battery-powered devices.
Speed is Increased: Communication between the main processor and memory is incredibly fast.
Size is Minimized: A complete system can fit into a device the size of your fingertip.

## Components present in typical SoC 

A System-on-Chip (SoC) functions as a complete electronic system packed onto a single die. It is designed around four interdependent categories of blocks that facilitate processing, storage, I/O, and communication.

### CPU 
TThe CPU is the core processing unit of the SoC, responsible for executing instructions and controlling overall system operation. Modern SoCs often employ heterogeneous multi-core architectures, combining high-performance cores for intensive workloads with low-power efficiency cores for background tasks. This big.LITTLE style of computing ensures a balance between performance and energy efficiency. The CPU may be based on ISAs such as ARM or RISC-V, and its design is tightly coupled with cache and memory subsystems to maintain high throughput.

###  Memory
The memory subsystem acts as the SoC’s data manager, providing fast and reliable storage for program execution. Closest to the CPU are on-chip memories such as caches and SRAM, which deliver low-latency access to frequently used data. Beyond this, memory controllers interface with larger off-chip memories like DRAM, handling timing protocols and ensuring efficient data transfer. Non-volatile memories such as ROM or flash are also integrated for boot code and firmware storage. The hierarchical memory design ensures the processor has the right balance of speed and capacity to operate efficiently.

### Peripherals and I/O Blocks 
Peripherals extend the SoC’s ability to interact with its environment. These include external interfaces such as USB, Wi-Fi, Bluetooth, camera controllers, and GPIOs, as well as internal utilities like timers, clock generators, and interrupt controllers. The interrupt controller is particularly important as it allows peripherals to signal the CPU only when needed, enabling efficient multitasking. In application-specific SoCs, specialized peripherals like image signal processors or baseband processors are included, highlighting the versatility and integration capability of SoC designs.

###  Interconnects
The interconnect provides the communication backbone of the SoC, linking the CPU, memory, and peripherals. While early designs relied on simple buses, modern SoCs employ advanced protocols like AXI, which support parallel transactions and high-speed burst transfers. For large, complex systems, Network-on-Chip (NoC) architectures are used, offering scalable, packet-based communication with mechanisms for arbitration and quality of service. This ensures smooth dataflow and prevents bottlenecks, maintaining system performance even under demanding workloads.

## VSDBabySoC Architecture
The VSDBabySoC is a compact, functional SoC built around the RISC-V architecture, designed specifically to validate and interface core IP blocks.
The chip's operation flows from the PLL (creating timing stability) to the RVMYTH (processing data) to the DAC (outputting the converted analog signal).
| Component | Function | Role in System |
| :--- | :--- | :--- |
| **RVMYTH CPU** | The processing core, based on open-source RISC-V. | Executes instructions and sequentially updates the **r17 register** to generate continuous data streams for analog output. |
| **PLL** | Phase-Locked Loop control system. | Generates a **stable, synchronized clock signal** from a reference frequency, essential for coordinating all digital and analog activities and mitigating off-chip timing issues (jitter, delays). |
| **10-bit DAC** | Digital-to-Analog Converter. | Converts the digital data from RVMYTH into a continuous **analog signal** (e.g., sound or video) for interfacing with external devices. |

### RVMYTH 
The RVMYTH core is built on the RISC-V (Reduced Instruction Set Computer - Five) architecture, typically implementing the RV32I (32-bit Integer) base instruction set. This modern, open ISA makes the core highly flexible and relevant for current industry trends.

RVMYTH typically implements a single-cycle or very simple multi-cycle pipeline. This structure breaks down instruction execution into discrete stages (Fetch, Decode, Execute, Memory, Write back).  It contains the standard 32 general-purpose 32-bit registers required by the RV32I specification (e.g., r0 is hardwired to zero, r17 is used for data output in BabySoC).

### PLL 
A Phase-Locked Loop (PLL) is a critical clock-generation circuit in the BabySoC, functioning as a sophisticated electronic system designed to maintain stable timing.Its fundamental theoretical purpose is to act as a closed-loop feedback control system that ensures the phase and frequency of its output signal are precisely locked to a clean, external reference signal. 

This synchronization is paramount for the entire SoC. The PLL achieves this synchronization through a continuous process involving three main blocks. First, the Phase Detector (PD) compares the phase of the output signal with the input reference, generating an error voltage proportional to the phase difference. This error voltage is then fed into the Loop Filter (LF), which smooths out high-frequency noise and stabilizes the control signal. Finally, the stable voltage controls the Voltage-Controlled Oscillator (VCO), which generates the actual output clock signal, adjusting its frequency until the phase lock is achieved. 

The PLL is integral to the BabySoC because it solves critical timing issues inherent to external clock distribution. It performs frequency multiplication (e.g., the 8x operation) to create the high-speed clock required by the RVMYTH CPU from a slow, stable source. Crucially, the PLL actively reduces Clock Jitter (undesired timing variations) and mitigates manufacturing inaccuracies (like ppm error) from external crystals. By providing a highly clean and synchronized internal clock, the PLL ensures the data integrity of all sequential transfers across the SoC.

### DAC
The Digital-to-Analog Converter (DAC) is the critical mixed-signal bridge in the BabySoC, responsible for translating the digital output of the RVMYTH CPU into a continuous, usable analog signal for the external world. Its core function is signal conversion: it takes the discrete binary codes (0s and 1s) from the processor's registers (like r17) and transforms them into a proportional output voltage or current.

In the VSDBabySoC, the DAC is typically a 10-bit device, meaning it can resolve 1,024 distinct analog levels, which defines the precision of the generated waveform (e.g., sound or video). The internal design often utilizes a highly scalable structure like the R-2R Ladder DAC for accurate weighted conversion. By completing this digital-to-analog step, the DAC allows the BabySoC to function as a complete embedded system, enabling it to drive multimedia outputs (speakers, displays) and interface directly with the analog environment.
