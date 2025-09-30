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
The Central Processor (CPU) is undeniably the "Brain" of the SoC. Its primary function is to execute all the software instructions and manage the flow of the entire system.

In modern SoCs, the CPU subsystem rarely relies on a single core. Instead, it uses Heterogeneous Computing (different types of cores working together). This typically involves:
Performance Cores (Big Cores): These are high-power cores dedicated to handling demanding tasks like running complex applications, intense calculations, or gaming.
Efficiency Cores (Little Cores): These are low-power cores that manage background tasks, system monitoring, and simple data processing. This setup allows the SoC to optimize for battery life by only waking up the "Big Cores" when high performance is genuinely needed.
