+++
weight = 50
+++

## ECCCADDR0 [[6]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___ecccaddr0.html)

![](/eccaddr0.png)

---

## ECCCADDR1 [[7]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___ecccaddr1.html)

![](/eccaddr1.png)

---

Step back again!

---

## DDR4 top level [[8]](https://www.systemverilog.io/ddr4-basics)

![](/ddr4-basics-top-level.png)

---

## DDR4 banks [[8]](https://www.systemverilog.io/ddr4-basics)

![](/ddr4-basics-banks.png)

---

## DDR4 row column [[8]](https://www.systemverilog.io/ddr4-basics)

![](/ddr4-basics-row-col.png)

---

## DDR4 rank (depth cascading) [[8]](https://www.systemverilog.io/ddr4-basics)

![](/ddr4-basics-rank.png)

---

## DDR4 width cascading [[8]](https://www.systemverilog.io/ddr4-basics)

![](/ddr4-basics-width-cascade.png)

---
## DDR4 address bus topology [[9]](https://www.rambus.com/fly-by-command-address/)

![](/ddr-addr-bus.png)

---


## From logical to physical address

* 16GB = 34bits = 17179869184
* 64 Bytes bursts = 8 x (8 Bytes words = 64 bits)
* Bits [2:0] unused -> burst size
* 31 bits left to map
* Rank, Bank Group, Bank, Row, Column

---

## ADDRMAP[0-11] Registers

* Contain the information to map AXI addresses to Rank, Bank Group, Bank, Row, Column

---

## ADDRMAP Registers [[10]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___addrmap0.html)
![](/addrmap0.png)

---

## From logical to physical address (again)

```sh
33: rank  0
32: row  15
|     |
17: row   0
16: bank  1
15: bank  0
14: bg    1
13: col   9
|         |
7:  col   3
6:  bg    0
5:  col   2
|         |
3:  col   0
```

---

## ECCCADDR1 [[7]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___ecccaddr1.html)

![](/eccaddr1.png)

---

## Column Adress and beat number [[11]](https://forums.xilinx.com/t5/Memory-Interfaces-and-NoC/ECCCADDR1-in-DDRC-Module/td-p/924982)

* 12 bits column address
* 8 beats of data for each command (x 64 bits = 64 bytes)
* Bits [2:0] of the column will always be 0
* If col[2:0] = 0 => data beats 0-3
* If col[2:0] = 4 => data beats 4-7

---

## Beat Number [[12]](https://www.xilinx.com/html_docs/registers/ug1087/ddrc___eccstat.html)

![](/eccstat.png)

---

## Address Range

* The address space is not contiguous on the zynqmp
* 0-0x80000000 = 2GB
* 0x800000000-B80000000 = 14GB
* Peripherals mapped to the top of 32 bits addresses
* Needed for the 32 bits cortex-R5 + pmu(microblaze) access
* Another translation is needed

---
## Tests

* Now we are able to get the address of the ECC error
* We are also able to poison an address
* More testing
* ECC errors actually happen in bursts (they do not accumulate)
* They happen after a few hours
* It really looks a HW issue
