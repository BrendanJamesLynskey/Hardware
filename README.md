# Hardware

A collection of synthesisable RTL designs, power electronics projects, and educational resources covering CPU architecture, arithmetic hardware, finite field (Galois Field) implementations, SoC design, and DC-DC converter control.

## ▶ [Open the Hardware Landing Page](https://brendanjameslynskey.github.io/Hardware/)

> **Setup:** Enable GitHub Pages (Settings → Pages → Deploy from `main` branch, `/ (root)` directory).
>
> Alternatively, open `index.html` locally in any browser — all presentations work offline after first load.

---

## Interactive Presentations

| Topic | Description |
|-------|-------------|
| [RISC-V — History, ISA &amp; Ecosystem](https://brendanjameslynskey.github.io/RISC_V/) | 29-slide deep dive on RISC-V: origins at Berkeley, base ISAs (RV32I/64I/32E/128I), the full extension alphabet (M·A·F·D·C·B·K·V·H), privilege modes, virtual memory, traps, RVV 1.0 vectors, hypervisor, custom extensions &amp; CHERIoT, vendor landscape, and the modern software ecosystem — with an **interactive instruction-format encoder** and a **mini RV32I CPU stepper** running Fibonacci |
| [Arm Cortex-M — 8-part Presentation Series](https://brendanjameslynskey.github.io/Cortex_M/) | Eight Reveal.js decks (~195 slides total) covering the Cortex-M family end-to-end: history of Arm (Acorn → ARM1 → Armv9), Cortex-M family (M0 → M85), Armv6-M / v7-M / v8-M architecture, programmer's model, NVIC exception model, memory system &amp; MPU, DSP / FPU / Helium (MVE), CoreSight debug &amp; trace, low-power design, and TrustZone for Armv8-M — with interactive core-picker and architecture-version picker |
| [Arm AMBA — 6-part Presentation Series](https://brendanjameslynskey.github.io/AMBA/) | Six Reveal.js decks (~129 slides total) covering Arm's Advanced Microcontroller Bus Architecture end-to-end: history of AMBA (1996 → 2024), AHB &amp; APB protocols, AXI deep dive (channels, bursts, IDs, ordering, QoS, Lite &amp; Stream), ACE &amp; ACE-Lite coherency, CHI scalable multi-cluster coherency (CMN-600/650/700), and the future (CHI-C2C, UCIe chiplets, CXL, MPAM, RME / CCA, AI accelerators) — with interactive AMBA-generation and burst-type pickers, plus SystemVerilog snippets highlighting minimal-hardware patterns (AXI4-Stream wire-loopback, APB one-register slaves, ACE-Lite constant tie-offs, skid-buffer pipeline stage) |
| [Arm Cortex-A — 6-part Presentation Series](https://brendanjameslynskey.github.io/Cortex_A/) | Six Reveal.js decks (~104 slides total) on the Arm application-profile family end-to-end: history &amp; family (ARM11 → A9 → A15 + big.LITTLE → A53/A57 AArch64 pivot → A76/A78 → Cortex-X1 → X925), Armv8-A / Armv9-A architecture &amp; Exception Levels, memory system (VMSA, TLBs, caches, weak ordering, LSE atomics), vector extensions (NEON · SVE · SVE2 · SME), security (TrustZone-A · PAC · BTI · MTE · RME / CCA), and microarchitecture (OoO, branch prediction, DynamIQ, PMU / SPE / TRBE) — with interactive Cortex-A core picker and EL selector |
| [Arm Neoverse — 5-part Presentation Series](https://brendanjameslynskey.github.io/Neoverse/) | Five Reveal.js decks (~80 slides total) on Arm's infrastructure-class CPU family: history &amp; product lines (N1 / V1 / E1 / N2 / V2 / N3 / V3), microarchitecture &amp; server-class RAS, the CMN-600 / 650 / 700 / S3 mesh interconnect (RN-F / HN-F / SN-F / SLC slicing / CHI), SBSA + SystemReady + the platform contract (ACPI, PSCI, TF-A, EDK2), and the ecosystem — AWS Graviton 1-4, Ampere Altra / AmpereOne, NVIDIA Grace-Hopper, Microsoft Cobalt, Alibaba Yitian, SiPearl Rhea, Fujitsu A64FX — with interactive Neoverse-silicon picker |
| [Arm System IP — 4-part Presentation Series](https://brendanjameslynskey.github.io/Arm_System_IP/) | Four Reveal.js decks (~64 slides total) on the system IP that surrounds every Arm CPU: GIC (v2/v3/v4, GIC-400/500/600/700, SPI/PPI/SGI/LPI, ITS, vGIC), SMMU (v2/v3, SMMU-600/700, stream tables, stages 1+2, ATS/PASID/PRI, IORT), DynamIQ Shared Unit (DSU-110/120/120AE, shared L3, per-core DVFS, CHI egress), and MPAM + CoreSight (memory-system partitioning, Linux resctrl, DAP/ETM/ITM/STM/CTI/TRBE) |
| [Introduction to Simulation](https://brendanjameslynskey.github.io/Introduction_to_Simulation/) | 30-slide on-ramp to simulation as engineers practise it, from field solvers to cloud systems: physics and numerical methods (FDTD, FEM, MoM, the CFL condition, CFD, TCAD), circuits (SPICE's MNA, Newton–Raphson and timestep control; switching simulators; IBIS and IBIS-AMI), digital logic (event-driven against cycle-based, with Icarus and Verilator timed on the same RTL; gate level, emulation and FPGA prototypes), architecture (roofline, discrete-event, SystemC TLM, cycle-level), system, network and Monte Carlo simulation, and virtual platforms; then the cross-cutting methods: time-stepping against events, stiffness and step control, randomness and replications, co-simulation and HIL, digital twins, verification and validation, a speed–accuracy chart of every level and a decision table. With an **interactive four-solver demo** (forward and backward Euler, adaptive Dormand–Prince with and without breakpoints, an event-driven exact solver) and links at every level into the decks and simulators below |
| [Free SystemVerilog Simulators — What Actually Runs](https://brendanjameslynskey.github.io/SystemVerilog_Simulators/) | 15-slide comparison of Icarus Verilog 12, Verilator 5.020, the AMD Vivado simulator (xsim), Questa–Altera Starter and the slang front end: a construct-by-construct support matrix with every cell tagged observed or documented, the exact error messages and workarounds from running nineteen RTL interview challenges, language mistakes every tool should reject, testbench timing pitfalls, and an **interactive “which simulator runs my testbench?” picker** |
| [Equalisation in High-Speed Serial Links](https://brendanjameslynskey.github.io/SerDes_Equalisation/) | One 28 GBd backplane channel taken end to end — S-parameters, the pulse response, CTLE / transmit FFE / receive FFE / DFE, a noise budget that lands 1.36 dB short of 10<sup>&minus;12</sup>, and seven remedies priced in dB. Covers the standards taxonomy (PCIe, IEEE 802.3, OIF CEI), laminate materials, PAM4 and FEC, chiplets, serial memory and CXL — with interactive via-stub and full-equaliser-chain widgets. Long-form PDF alongside |
| [FHE Accelerator Simulators — 5-part Presentation Series](https://brendanjameslynskey.github.io/FHE_Hub_Accelerator_Simulators/) | Five interactive decks on simulating a fully-homomorphic-encryption accelerator running CKKS bootstrapping: CKKS as a hardware workload (RNS polynomials, key and ciphertext sizes, NTT / base conversion / automorphism kernels, key switching), the anatomy of bootstrapping stage by stage, a SimPy accelerator model with a **live in-browser simulator**, optical NTT engines (digit planes, ENOB, Walden converter energy), and design-space results (SRAM against key traffic, NTT-bound vs memory-bound vs power-bound). Companion code: [FHE_Accelerator_Sim](https://github.com/BrendanJamesLynskey/FHE_Accelerator_Sim) (SimPy, 104 tests, an optional command-level HBM model, a dynamic power manager, OpenFHE calibration and replayed OpenFHE bootstrap traces, a HEIR compiler front end, SlotToCoeff-first ordering, bit-exact JavaScript port); a [glossary](https://brendanjameslynskey.github.io/FHE_Hub_Accelerator_Simulators/#glossary) links every concept to the slides that explain it |
| [Fourier Optics for Inference — 3-part Presentation Series](https://brendanjameslynskey.github.io/LLM_Hub_Fourier_Optics_Inference/) | Three interactive decks on whether Fourier optics could accelerate disaggregated LLM inference: Fourier optics for engineers (the lens as a Fourier transformer, the 4f system, coherence and the phase problem, SLMs, ENOB, averaging passes, Walden conversion energy, with a 1-D/2-D FFT-convolution demo); where transforms appear in inference workloads, with an **Amdahl analysis of prefill FLOP shares** from a published script; and **optical prefill pools, simulated** — Disaggregated_Inference_Sim extended with heterogeneous pools, FFT-mixing models, an optical transform engine and KV hand-off compression (in transit or at the GPU), with break-even points, PPA and a **live in-browser simulator**. The honest answer is mostly negative, and the decks show where the line is |
| [Simulation Engineering Toolkit](https://brendanjameslynskey.github.io/SimEng_Hub_Toolkit/) | The engineering around a hardware-architecture simulator: Rust for simulation engineers, a bit-exact Rust/PyO3 port of a SimPy simulator, **C++ performance models in SystemC TLM-2.0** (AT and LT, op-by-op agreement with a SimPy model), **DRAM/HBM memory-system modelling** (FR-FCFS, address mapping, refresh, a DRAMsim3 cross-check), a **verification bridge** (cocotb on Verilator, golden models, coverage crosses, RTL cycles calibrating the simulator), testing frameworks for simulators, **Jenkins** for hardware and simulation teams (real runs of all five code repos), Jira and engineering metrics, specifications, requirements and test plans (EARS, a generated traceability matrix), **PyTorch and ONNX front ends** for an accelerator model, performance analysis (flame graphs, cachegrind, regression gates), and **measurement tools and methods** (profilers, hardware counters, GPU and CPU energy telemetry, RTL coverage and power flows, DRAM simulators, and the CACTI, McPAT and Accelergy estimators, with a CACTI SRAM sweep at 22 nm), and **power, performance and area** (dynamic and static power, die area and yield, perf/W, perf/mm² and EDP, three-way Pareto fronts on an FHE accelerator area model), and **an accelerator model in SimPy, end to end** (DMA, on-chip buffer with back-pressure, compute array, stall attribution and timelines, a cycle-stepped twin, a C++/pybind11 fast path). Fourteen decks. Companion code: [Rust_DES_Kernel](https://github.com/BrendanJamesLynskey/Rust_DES_Kernel), [SystemC_Accelerator_Model](https://github.com/BrendanJamesLynskey/SystemC_Accelerator_Model), [Memory_System_Sim](https://github.com/BrendanJamesLynskey/Memory_System_Sim), [RTL_CoSim_NTT](https://github.com/BrendanJamesLynskey/RTL_CoSim_NTT), [Torch_Sim_Frontend](https://github.com/BrendanJamesLynskey/Torch_Sim_Frontend); a [glossary](https://brendanjameslynskey.github.io/SimEng_Hub_Toolkit/#glossary) links every concept to the slides that explain it |

---

## RISC-V SoC Platform

A complete RISC-V System-on-Chip built from independently verified subsystems.

| Project | Description |
|---------|-------------|
| [RISC-V SoC — System Integration](https://github.com/BrendanJamesLynskey/RISCV_SoC) | **Top-level SoC** integrating CPU, crossbar, MMU, cache, DMA, and IOMMU into a unified bus-connected system with PLIC, SRAM, and peripheral bridge |

### SoC Subsystems

| Project | Role in SoC | Description |
|---------|-------------|-------------|
| [BRV32P — 5-Stage Pipelined RV32IMC](https://github.com/BrendanJamesLynskey/RISCV_RV32IMC_5stage) | CPU Core | In-order 5-stage pipeline with full forwarding, 2-way set-associative caches, AXI4-Lite bus, branch prediction, M and C extensions |
| [AXI4 Crossbar](https://github.com/BrendanJamesLynskey/AXI4_Crossbar) | Interconnect | Parameterised NxM AXI4 crossbar — round-robin arbitration, ID-based response routing, error slave, independent read/write paths |
| [MMU (Sv32)](https://github.com/BrendanJamesLynskey/MMU) | Virtual Memory | Sv32 MMU with fully-associative TLB, hardware page table walker, permission checking, SFENCE.VMA support |
| [Cache Controller (MESI)](https://github.com/BrendanJamesLynskey/Cache_Controller_MESI) | L2 Cache | 4-way set-associative cache with MESI coherence, write-back policy, snoop interface, AXI4 bus interface |
| [DMA Controller](https://github.com/BrendanJamesLynskey/RISCV_DMA) | DMA Engine | 4-channel DMA with scatter-gather, AXI4 master, per-channel interrupts |
| [IOMMU (Sv32)](https://github.com/BrendanJamesLynskey/RISCV_IOMMU) | I/O Translation | I/O address translation for DMA isolation — IOTLB, device context cache, fault handling |
| [PLIC](https://github.com/BrendanJamesLynskey/RISCV_PLIC) | Interrupt Controller | Platform-Level Interrupt Controller — priority-based arbitration, configurable enable/threshold, claim/complete |

---

## RISC-V CPUs

| Project | Description |
|---------|-------------|
| [BRV32 — Single-Cycle RV32I](https://github.com/BrendanJamesLynskey/RISCV_RV32I_SingleCycle) | Complete single-cycle RV32I SoC in Verilog-2001 — CPU, GPIO, UART, Timer, machine-mode CSRs. 32/32 tests passing |
| [RISC-V Presentation](https://github.com/BrendanJamesLynskey/RISC_V) | 29-slide interactive Reveal.js deck on RISC-V — origins, base ISAs, standard extensions, privilege model, virtual memory, traps, RVV 1.0, hypervisor, custom extensions &amp; CHERIoT, vendors and software ecosystem, with interactive instruction-format encoder and mini RV32I CPU stepper. [Live on GitHub Pages.](https://brendanjameslynskey.github.io/RISC_V/) |

## Arithmetic Units

| Project | Description |
|---------|-------------|
| [Integer Dividers](https://github.com/BrendanJamesLynskey/Integer_dividers) | Five SystemVerilog divider architectures — restoring, non-performing, non-restoring, SRT radix-4, and Newton-Raphson |
| [Floating-Point Dividers](https://github.com/BrendanJamesLynskey/Floating_Point_Dividers) | Six IEEE 754 FP32 divider architectures — restoring, non-restoring, SRT-2, SRT-4, Newton-Raphson, and Goldschmidt |
| [CORDIC](https://github.com/BrendanJamesLynskey/CORDIC) | Synthesisable SystemVerilog implementations of the CORDIC algorithm |
| [Neural Network Data Types](https://github.com/BrendanJamesLynskey/NN_data_types) | SystemVerilog implementations of 9 numerical formats (FP32 down to FP4) used in NN training and inference hardware |

## Finite Fields (Galois Fields) in Hardware

| Project | Description |
| --- | --- |
| [Galois Fields in Hardware](https://github.com/BrendanJamesLynskey/Galois_Fields) | Interactive Reveal.js article on Galois Field (GF) theory for hardware engineers — GF(2) and GF(2^n) arithmetic, irreducible polynomials, and applications in CRC, LFSR, AES, and error-correcting codes, with interactive graphics |
| [CRC — Cyclic Redundancy Checks](https://github.com/BrendanJamesLynskey/CRC) | SystemVerilog CRC implementations — bit-serial LFSR, byte-parallel table-based, and byte-parallel XOR-tree architectures with parameterised polynomials and self-checking testbenches |
| [LFSR — Linear Feedback Shift Registers](https://github.com/BrendanJamesLynskey/LFSR) | SystemVerilog LFSR implementations — Fibonacci and Galois configurations, PRBS generators (PRBS-7/15/31), and additive scrambler/descrambler pairs with self-checking testbenches |

## ML Accelerator Hardware

| Project | Description |
|---------|-------------|
| [Transformer Decoder — RTL Accelerator](https://github.com/BrendanJamesLynskey/LLM_Transformer_Decoder_RTL) | Synthesisable SystemVerilog implementation of a pre-norm decoder block with KV-cache, plus full verification suite (83 tests) |
| [AI Matrix Multiplier Units (MMUL)](https://github.com/BrendanJamesLynskey/AI_MMUL_Unit) | Hardware deep-dive on the MMUL — the kernel of every AI accelerator. 24-slide interactive presentation covering history (Kung 1978/82 → TPU → V100 → Blackwell), four dataflow architectures (output-/weight-/input-/row-stationary, dot-product trees), Transformer mapping (Q/K/V/FFN GEMMs), number formats (FP32 → FP4, MXFP, NVFP4), real systems (TPU/Trillium, H100/B200, Cerebras WSE-3, Groq, Dojo, Trainium2, ANE, AMX, SME, Sohu, Tenstorrent), memory hierarchy & caches, performance, power and thermals — with four parameterised SystemVerilog implementations, a Python golden model, and 258 passing tests (SV + CocoTB) |
| [Systolic Arrays Explained](https://github.com/BrendanJamesLynskey/systolic-arrays-explained) | Interactive web app at [systolic-arrays-explained.vercel.app](https://systolic-arrays-explained.vercel.app/): the matrix hardware under the models in 9 animated chapters — the memory wall, weight-/output-/input-stationary cycle by cycle, skew, fill and drain, tiling with double-buffered weights, inside a PE (bfloat16/FP16/INT8 MAC pipeline), the TPU and a torus all-reduce, row-stationary and tensor cores, an ONNX graph lowered onto the array. Every frame comes from a cycle-accurate model (Python reference, exact TypeScript port) checked against the author's SystemVerilog array and MAC unit in Verilator |

## Power Electronics

| Project | Description |
|---------|-------------|
| [DC-DC Converter Control Techniques](https://github.com/BrendanJamesLynskey/DCDC_Control_Techniques) | Interactive Reveal.js presentation covering PWM (voltage-mode, peak/valley/average current-mode), PFM, hysteretic, and constant on-time (COT) control — with interactive waveform and efficiency graphics, tradeoff comparisons, and future directions including digital control and GaN |
| [COT DC-DC Converter](https://github.com/BrendanJamesLynskey/COT_DCDC_Simulink) | MATLAB/Simulink constant on-time DC-DC converter model, adapted from the NPTEL course on switched mode power converter control |

## Signal Integrity &amp; High-Speed Digital Design

Material on getting a signal from one chip to another intact — the physics of the channel,
the equalisation that rescues it, and the layout practice that decides how much rescuing is
needed.

| Project | Description |
|---------|-------------|
| **[Signal Integrity &amp; High-Speed Digital Design](https://github.com/BrendanJamesLynskey/Signal_Integrity)** | **The main series: seventeen interactive decks, 188 slides, in five sections.** The passive channel (transmission lines, return paths, materials and causality, vias and back-drilling); the signal on it (differential signalling, crosstalk); jitter, in four decks built on Ransom Stephens's rules; power integrity, in four decks built on Smith and Bogatin's; and then timing budgets, measurement and de-embedding, and channel operating margin. Every number computed by a model and checked against published worked examples |
| [Equalisation in High-Speed Serial Links](https://github.com/BrendanJamesLynskey/SerDes_Equalisation) | A 28 GBd backplane channel worked from S-parameters to a closed link budget: pulse response, CTLE / FFE / DFE, noise budget, PAM4 and FEC, chiplets, serial memory and CXL. Interactive channel and equaliser widgets, plus a long-form PDF |
| [Matrix Methods in Network Parameters](https://github.com/BrendanJamesLynskey/Matrix_Methods_Network_Parameters) | The S-, Z- and Y-parameter theory the channel description rests on — reciprocity, passivity, losslessness, mixed-mode and skew-driven mode conversion, causality and the Smith chart |
| [Matrix Concepts in Digital Filters](https://github.com/BrendanJamesLynskey/Matrix_Concepts_Digital_Filters) | The filter theory the equalisers rest on — state-space stability, Wiener–Hopf optimal taps, the eigenfilter, paraunitary banks |
| [Kramers–Kronig Relations](https://github.com/BrendanJamesLynskey/Kramers_Kronig_Relations) | Causality and the analyticity that ties magnitude to phase — the constraint every fitted channel model has to honour |
| [High-Speed Serial Links — interview preparation](https://github.com/BrendanJamesLynskey/Interview_High_Speed_Serial_Links) | Written notes and worked problems: link budgets, driver architectures, PLL jitter, CTLE / DFE / CDR, PCIe Gen5-6, UCIe, NVLink, eye analysis, crosstalk and PDN coupling |
| [LPDDRx Layout — interview preparation](https://github.com/BrendanJamesLynskey/Interview_LPDDRx_Layout) | The parallel-bus side of the same problem: memory interface layout, skew, and termination |
| [Matrix Articles](https://github.com/BrendanJamesLynskey/Matrix_Articles) | The sources and computed models behind all of the above — the channel model, the 2-D field solver, and one module per signal-integrity deck |

## Accelerator Modelling and Verification

Code companions of the [Simulation Engineering Toolkit](https://brendanjameslynskey.github.io/SimEng_Hub_Toolkit/), built around the FHE accelerator model above.

| Project | Description |
|---------|-------------|
| [RTL_CoSim_NTT](https://github.com/BrendanJamesLynskey/RTL_CoSim_NTT) | SystemVerilog Barrett multiplier, NTT butterfly and P-lane NTT core, verified with cocotb on Verilator and Icarus against FHE_Accelerator_Sim's NTT as the golden model; constrained-random stimulus with functional-coverage crosses, a seeded bug that uniform stimulus misses, an exact cycle model, and the measured NTT efficiency fed back into the simulator |
| [Memory_System_Sim](https://github.com/BrendanJamesLynskey/Memory_System_Sim) | Command-level DRAM/HBM timing simulator (bank state machines, tRCD/tRP/tFAW/tCCD/refresh, address mapping with XOR hashing, FCFS and FR-FCFS) with an independent protocol checker, Hypothesis traces and a DRAMsim3 cross-check; plugs into FHE_Accelerator_Sim as its HBM model |
| [SystemC_Accelerator_Model](https://github.com/BrendanJamesLynskey/SystemC_Accelerator_Model) | C++17 SystemC TLM-2.0 model of the FHE accelerator tile, approximately- and loosely-timed, driven by FHE_Accelerator_Sim's traces and matching its SimPy model op by op; GoogleTest with ASan/UBSan |

## SoC Design

| Project | Description |
|---------|-------------|
| [Modern SoC Design](https://github.com/BrendanJamesLynskey/SoC) | Interactive presentation series — advanced packaging, chiplets, on-chip interconnect/NoC, memory hierarchies, and high-speed SerDes. Decks 01, 04, 08 and 09 sit directly alongside the signal-integrity material above |

## HDL Examples

| Project | Description |
|---------|-------------|
| [VHDL Example Code](https://github.com/BrendanJamesLynskey/VHDL_example_code) | AHB-to-MII Ethernet MAC (10/100) with AMBA AHB host interface, testbench, and verification plans — authored 2007–2008 as the author's MSc project in Low Power Systems Integration at the University of Manchester (completed with distinction); contributed as an I/O block to the SpiNNaker neuromorphic chip |
