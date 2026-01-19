# pynqz2-rsa-montgomery-accelerator

Hardware-accelerated Montgomery modular multiplication for RSA on the Xilinx PYNQ-Z2 platform.

This repository presents a complete hardware–software co-design that accelerates
2048-bit Montgomery multiplication using an FPGA-based accelerator integrated with
the ARM Cortex-A9 processing system via AXI4-Lite. The accelerator is evaluated using
RSA-1024 and RSA-2048 encryption and decryption benchmarks.

📌 Course Project for **ECE4300 (Fall 2025)**  
📍 California State Polytechnic University, Pomona

---

## Project Overview

RSA encryption and decryption rely heavily on large-integer modular multiplication.
Montgomery multiplication eliminates expensive division operations and is well suited
for FPGA acceleration.

In this project, we accelerate **only the Montgomery multiplication kernel** in hardware
while keeping the rest of the RSA algorithm in software. Despite this partial offloading,
the system achieves significant end-to-end RSA performance improvements on a
resource-constrained SoC platform.

### Key Results
- **RSA-2048 Encryption:** ~7.5× speedup  
- **RSA-2048 Decryption:** ~12.5× speedup  
- Similar performance gains observed for RSA-1024  
- All hardware results match software outputs bit-accurately

---

## Repository Structure

├── montgomery_mul.v # 2048-bit Montgomery multiplier (Verilog)
├── montgomery_axi.v # AXI4-Lite interface wrapper
├── main_1.c # Software implementation and benchmarks
├── Final_Report.pdf # Final report
├── Project Overview.pdf # Project summary
└── README.md


---

## Hardware Design

### Montgomery Multiplier

- Supports 2048-bit operands (64 × 32-bit words)
- Iterative, word-serial architecture
- Controlled by an internal finite state machine
- Results verified against the software implementation

### AXI Interface

The accelerator is accessed from the ARM processor through AXI4-Lite registers.
The processor writes operands and parameters, starts the operation, polls for
completion, and then reads back the result.

---

## Software Implementation

The software (`main_1.c`) includes:
- A pure software Montgomery multiplication baseline
- Control code for the hardware accelerator
- Correctness checking using known test vectors
- RSA-1024 and RSA-2048 performance benchmarks
- Timing measurements using `clock_gettime()`

---

## Results

### RSA-2048

| Operation | Software Cycles | Hardware Cycles | Speedup |
|----------|-----------------|-----------------|---------|
| Encrypt  | ~6.08×10⁸ | ~7.98×10⁶ | ~7.5× |
| Decrypt  | ~1.49×10⁸ | ~1.20×10⁷ | ~12.5× |

### RSA-1024

| Operation | Software Cycles | Hardware Cycles | Speedup |
|----------|-----------------|-----------------|---------|
| Encrypt  | ~1.06×10⁸ | ~1.97×10⁶ | ~7.5× |
| Decrypt  | ~4.06×10⁸ | ~3.61×10⁶ | ~11.4× |

All hardware results match the software results for all tested cases.

---

## Authors

Caleb Pai  
Winson Zhu  
Paul Kim  
Hyukjin Jeong  

Department of Electrical & Computer Engineering  
California State Polytechnic University, Pomona

