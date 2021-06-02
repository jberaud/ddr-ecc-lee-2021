+++
weight = 70
+++

## Main Causes of DDR Failures

* Temperature
* Power
* Signal Integrity

---

## Tests

* Temperature measurements showed no correlation with DDR ECC errors
* Power sources were inspected by the Hardware team without finding anything
* Generation of a version with a frequency lowered to 600 MHz did make the issue disappear
* Lowering the frequency to 1000 MHz has also made the errors disappear

---

## So what was the issue ?

---

## Address bus

![](/ddr-addr-bus.png)

---

## Line Termination

* Coherent with the address patterns that made the ECC Errors appear
