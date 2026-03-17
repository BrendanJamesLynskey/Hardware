# Hardware

A collection of synthesisable RTL designs, power electronics projects, and educational resources covering CPU architecture, arithmetic hardware, SoC design, and DC-DC converter control.

---

## RISC-V CPUs

| Project | Description |
|---------|-------------|
| [BRV32 — Single-Cycle RV32I](https://github.com/BrendanJamesLynskey/RISCV_RV32I_SingleCycle) | Complete single-cycle RV32I SoC in Verilog-2001 — CPU, GPIO, UART, Timer, machine-mode CSRs. 32/32 tests passing |
| [BRV32P — 5-Stage Pipelined RV32IMC](https://github.com/BrendanJamesLynskey/RISCV_RV32IMC_5stage) | In-order 5-stage pipeline with full forwarding, 2-way set-associative caches, AXI4-Lite bus, branch prediction, M and C extensions |

## Arithmetic Units

| Project | Description |
|---------|-------------|
| [Integer Dividers](https://github.com/BrendanJamesLynskey/Integer_dividers) | Five SystemVerilog divider architectures — restoring, non-performing, non-restoring, SRT radix-4, and Newton-Raphson |
| [Floating-Point Dividers](https://github.com/BrendanJamesLynskey/Floating_Point_Dividers) | Six IEEE 754 FP32 divider architectures — restoring, non-restoring, SRT-2, SRT-4, Newton-Raphson, and Goldschmidt |
| [CORDIC](https://github.com/BrendanJamesLynskey/CORDIC) | Synthesisable SystemVerilog implementations of the CORDIC algorithm |
| [Neural Network Data Types](https://github.com/BrendanJamesLynskey/NN_data_types) | SystemVerilog implementations of 9 numerical formats (FP32 down to FP4) used in NN training and inference hardware |

## ML Accelerator Hardware

| Project | Description |
|---------|-------------|
| [Transformer Decoder — RTL Accelerator](https://github.com/BrendanJamesLynskey/LLM_Transformer_Decoder_RTL) | Synthesisable SystemVerilog implementation of a pre-norm decoder block with KV-cache, plus full verification suite (83 tests) |

## Memory Management

| Project | Description |
|---------|-------------|
| [MMU — Sv32 Memory Management Unit](https://github.com/BrendanJamesLynskey/MMU) | Synthesisable SystemVerilog Sv32 MMU with TLB, hardware page table walker, permission checking, and SFENCE.VMA support. Full SV + CocoTB verification suite (131 tests) |

## Interrupt Controllers

| Project | Description |
|---------|-------------|
| [RISCV_PLIC](https://github.com/BrendanJamesLynskey/RISCV_PLIC) | RISC-V Platform-Level Interrupt Controller — parameterised, synthesisable, edge/level gateways, claim/complete, memory-mapped registers |

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
