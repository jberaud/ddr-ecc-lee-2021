+++
weight = 10
+++

## Fermat

Hardware + Software company based in Paris and Basel

> "An analytic engine fully integrated with high capacity Flash-based Storage"

---

## Introduction

---

## Top 2 FPGA manufacturers

* Altera (Intel)
* Xilinx (AMD)

---

## MicroBlaze - NIOS

* Soft-Core processor IPs to be able to run software on the FPGAs
* Customers making extensive use of those for various applications
* Linux ports for both architectures : `arch/nios` and `arch/microblaze`
* A bit experimental

---
 
## MPSoC -SoCFPGA

* ARM Cortex-A cores
* Supported by the mainline Linux Kernel (some patches on separate trees)
* Development Kits to generate lightweight BSP and HAL
* FreeRTOS or Standalone (one core)

---

## Xilinx Ultrascale+

![](/mpsoc-resized.png)

---

## Standalone

* A sort of bare-metal application
* HAL
* Drivers for the SoC devices as well as Xilinx's IP
* For those looking for a very low overhead in terms of latency and complexity.
* Can be challenging
