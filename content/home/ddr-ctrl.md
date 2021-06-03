+++
weight = 40
+++

## DDR Controller Registers [[3]](https://www.xilinx.com/html_docs/registers/ug1087/ddr_qos_ctrl___qos_irq_status.html)

![](/qos_irq_status.png)

---

## DDR Controller Registers [[4]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___eccerrcnt.html)

![](/eccerrcnt.png)

---

* ERR and UNCRERR flags set to 1 => DDR ECC Error
* ecc_uncorr_err_cnt = 4
* ecc_corr_err_cnt = 12

---

Step back!

---

## Error Correcting Code Block [[5]](https://www.xilinx.com/support/documentation/user_guides/ug1085-zynq-ultrascale-trm.pdf)

* Encoder and decoder that
	1. can detect and correct single-bit errors
	2. can detect double-bit errors 

---

## Error Correcting Code Block [[5]](https://www.xilinx.com/support/documentation/user_guides/ug1085-zynq-ultrascale-trm.pdf)

![](/ddr-ecc-block.png)

---

## ECC Initialization

* The whole memory needs to be written at startup in order to initialize the ECC bits

---

## Register an interrupt on DDR_QOS_IRQ

```c
	XScuGic *gicInstPtr = getGicInstPtr();

	// Register interrupt to handle DDR ECC errors.
	if (!RegIntrInGic(DDR_QOS_IRQ_ID, DdrEccErrorHandler,
			gicInstPtr, 0, 1)) {
		return false;
	}

	// Enable interrupt for ECC errors.
	Xil_Out32(XPAR_PSU_DDR_QOS_CTRL_S_AXI_BASEADDR +
			DDR_QOS_IRQ_ENABLE_OFFSET,
			DDR_QOSUE_MASK | DDR_QOSCE_MASK);
```

---

## Other relevant registers

* ECCSTAT, ECCCLR, ECCERRCNT, ECCCADDR0, ECCCADDR1, ECCSYN0, ECCSYN1, ECCSYN2, ECCBITMASK0, ECCBITMASK1, ECCBITMASK2, ECCUADDR0, ECCUADR1, ECCUSYN0, ECCUSYN1, ECCUSYN2

---

## ECC poisoning [[6]](https://www.xilinx.com/html_docs/registers/ug1087/ug1087-zynq-ultrascale-registers.html#mod___ddrc.html)

![](/ecccfg1.png)

---

## ECC poisoning over JTAG

```tcl
# SWCTL = 0 (enable quasi-dynamic programming)
xsct% mwr 0xFD070320 0
# ECCCFG1 = 3 (1 bit error poisonning enabled)
xsct% mwr 0xFD070074 3
# SWCTL = 1 (disable quasi-dynamic programming)
xsct% mwr 0xFD070320 1
# Note that by default ECCPOISONADDR0 and 1 = 0
# So the poisoned address is 0
```

---

## ECC poisoning over JTAG

```tcl
xsct% mrd 0 8
       0:   DEADBEEF
       4:   DEADBEEF
       8:   DEADBEEF
       C:   DEADBEEF
      10:   DEADBEEF
      14:   DEADBEEF
      18:   DEADBEEF
      1C:   DEADBEEF
```

---

## ECC poisoning over JTAG

```tcl
xsct% mwr 0 0
xsct% mrd 0
```
---

## ECC poisoning over JTAG

```sh
*** Recoverable ECC error, total 1 ECC recoverable errors
*** 0 ECC unrecoverable errors
*** EccStat 0x100
*** CADDR0 : 0x0, CADDR1 : 0x0, 
*** CSYN0 : 0x0, CSYN1 : 0xDEADBEEF, CSYN2 : 0xDA
*** BITMASK0 : 0x0, BITMASK1 : 0x0, BITMASK3 : 0x1

```
