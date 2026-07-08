# 16-Point Fast Fourier Transform (FFT) Processor: ASIC Flow Design

## Executive Summary
This repository contains the complete RTL-to-GDSII implementation of a hardware-accelerated 16-point Radix-2 Fast Fourier Transform (FFT) processor. Designed to address the high latency and power consumption limitations of software-based FFT implementations, this ASIC design is optimized for real-time Digital Signal Processing (DSP) and embedded systems. The processor was developed using Verilog HDL and synthesized into a physical layout using the open-source OpenLane ASIC flow and the SkyWater 130nm Process Design Kit (PDK).

## Technologies & EDA Tools
* **Hardware Description Language (HDL):** Verilog
* **Simulation & Functional Verification:** Xilinx Vivado, ModelSim, GTKWave
* **Logic Synthesis:** Yosys
* **Physical Design (Place & Route):** OpenLane Flow
* **Process Design Kit (PDK):** SkyWater 130nm (Sky130)

## Hardware Architecture & Methodology
The architecture is inherently modular, ensuring scalability, reuse, and efficient routing across multiple FFT computation stages. 

### 1. Core Computational Blocks
* **Radix-2 Algorithm:** Decomposes the Discrete Fourier Transform (DFT) to minimize computational complexity.
* **Butterfly Computation Unit:** The primary arithmetic engine, executing complex addition and subtraction for both real and imaginary components.
* **Twiddle Factor Multipliers:** Dedicated, precision-optimized signed multiplication blocks for complex exponential coefficients.

### 2. Memory & Storage
* **Dual-Port Memory Block:** Facilitates organized, synchronous read/write operations for stage-wise intermediate data.
* **Lookup Tables (LUTs):** * *Twiddle Factor LUT:* Stores precomputed complex exponentials, avoiding real-time computation and improving speed.
  * *Address Generation LUT:* Generates appropriate read addresses for fetching data in the correct order.

### 3. FSM Control Logic
A robust Finite State Machine (FSM) synchronizes the entire datapath. It dictates multiplexer selections, stage transitions, memory access timing, and the data flow between modules to ensure stall-free execution.

## RTL-to-GDSII ASIC Flow (Physical Design)
The processor was driven through a complete physical design pipeline to generate a fabrication-ready GDSII layout.

* **Floorplanning:** Defined core silicon area and I/O pin placements for efficient utilization of silicon space.
* **Standard Cell Placement:** Synthesized standard cells were placed to reduce congestion and improve performance.
* **Clock Tree Synthesis (CTS):** Generated a clock distribution network to ensure proper clock delivery with minimal skew and delay.
* **Routing & Optimization:** Established interconnections between cells, followed by optimization to fix timing issues and reduce wire length.
* **GDSII Generation:** Converted the final physical design into a GDSII layout file for fabrication.

## Performance, Power, and Area (PPA) Results
Extensive synthesis exploration was conducted using Yosys to identify the optimal PPA configuration before committing to Place and Route (PnR). Seven distinct area-driven and delay-driven strategies were evaluated.

**Selected Configuration: AREA 1 Strategy**
* **Total Silicon Area:** 68,576 µm²
* **Power Consumption:** 3.47 mW, indicating efficient power usage suitable for low-power DSP applications.
* **Timing & Frequency:** Achieved positive slack, successfully satisfying both setup and hold timing constraints.
* **Trade-off Analysis:** AREA 1 was chosen over the AREA 0 baseline as it yielded the smallest footprint (28,820.1 µm²) while simultaneously exhibiting a slight reduction in signal delay (12.24 ns vs. 12.40 ns). 

## Repository Structure
├── src/                    # Verilog RTL source files (Butterfly, Multiplier, FSM, etc.)
├── openlane_runs/          # PnR configuration scripts and physical design summaries
├── docs/                   # PDF Report, FSM diagrams, and schematic visualizations
├── images/                 # Heatmaps (IR Drop, Routing Congestion) and GDSII layouts
└── README.md               # Project documentation

## Future Scope
* **Scalability:** Extend the RTL architecture to support 32-point and 64-point FFT computations.
* **Throughput Optimization:** Implement multi-stage pipelining and parallel processing architectures.
* **Low-Power Enhancements:** Integrate clock gating to further suppress dynamic power consumption.
* **Hardware Integration:** Tape-out for fabrication and post-silicon testing.
