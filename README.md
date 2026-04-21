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

## Power Electronics

| Project | Description |
|---------|-------------|
| [DC-DC Converter Control Techniques](https://github.com/BrendanJamesLynskey/DCDC_Control_Techniques) | Interactive Reveal.js presentation covering PWM (voltage-mode, peak/valley/average current-mode), PFM, hysteretic, and constant on-time (COT) control — with interactive waveform and efficiency graphics, tradeoff comparisons, and future directions including digital control and GaN |
| [COT DC-DC Converter](https://github.com/BrendanJamesLynskey/COT_DCDC_Simulink) | MATLAB/Simulink constant on-time DC-DC converter model, adapted from the NPTEL course on switched mode power converter control |

## SoC Design

| Project | Description |
|---------|-------------|
| [Modern SoC Design](https://github.com/BrendanJamesLynskey/SoC) | Interactive presentation series — advanced packaging, chiplets, on-chip interconnect/NoC, memory hierarchies, and high-speed SerDes |

## HDL Examples

| Project | Description |
|---------|-------------|
| [VHDL Example Code](https://github.com/BrendanJamesLynskey/VHDL_example_code) | Example VHDL coding samples |
