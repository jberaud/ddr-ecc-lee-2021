+++
weight = 60
+++

## How to make the ECC errors appear ?

---

## DRAM Test app

![](/dram-test.png)

---

No luck...

---

## Taking a look at the DRAM test

* Writes blocks of random data
* Contiguous increasing addresses
* What if the issue is on the address bus ?

---

## Writing a different ddr test

* Cache enabled or disabled ?

---

## Writing a different ddr test

* Disabling cache makes memory writes really inefficient
* Each write means
	1. reading a full burst (64 bytes)
	2. writing a full burst
* No instruction to actually read/write a full burst (LDP/STP reads/writes 2 x 64 bits words)
* So 64 bytes read and 64 bytes written for every 16 bytes you want to write
* Too slow...

---

## Writing a different ddr test

* Generate a random address in the address range
* Generate random values
* Write a full cache
* Flush it
* Invalidate it
* Read the data back (ECC errors only occur when reading)

---

## Writing a different ddr test

```c
static void TestOneCache(void)
{
	unsigned int i;
	volatile uint64_t *writeBuf = (uint64_t *) GenAddr();

	for (i = 0; i < NDOUBLEWORDS; i++)
		writeBuf[i] = GenVal();
	
	Xil_DCacheInvalidate();
	
	for (i = 0; i < NDOUBLEWORDS; i++)
		readBuf[i] = writeBuf[i];
}
```

