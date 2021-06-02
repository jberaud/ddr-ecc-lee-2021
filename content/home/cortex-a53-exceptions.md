+++
weight = 30
+++

## Bug

```sh
...
...
...
Synchronous Abort!
```
---

## Synchronous Abort

> In the ARMv7-A architecture, the prefetch abort, Data Abort and undef exceptions are separate items. In AArch64, all of these events generate a Synchronous abort. The exception handler may then read the syndrome and FAR registers to obtain the necessary information to distinguish between them. [[1]](https://developer.arm.com/documentation/den0024/a/AArch64-Exception-Handling/Synchronous-and-asynchronous-exceptions/Synchronous-aborts?lang=en)

---

## ESR_EL3 [[2]](https://developer.arm.com/documentation/ddi0500/j/System-Control/AArch64-register-descriptions/Exception-Syndrome-Register--EL3?lang=en#CIHIEHJG)

![](/esr_el3.png)

---

## ESR_EL3 = 0x96000004

```console
	Exception Class = 0b100101
	Instruction Length = 1
	ISS Valid = 0
	ISS = 4
```

----

## Exception Class [[2]](https://developer.arm.com/documentation/ddi0500/j/System-Control/AArch64-register-descriptions/Exception-Syndrome-Register--EL3?lang=en#CIHIEHJG)

![](exception_class.png)

---

## Other information

* Randomly after a few hours
* ELR_EL3 at different locations in the code
* FAR_EL3 at different addresses that look perfectly valid

---

## Breakpoint

![](/question-mark-add.png)

---

Could very well be a DDR ECC error...
