# RISC-V

### TABLE OF CONTENTS
- [Day-1 - Introduction to RISC-V ISA and GNU compiler toolchain](#day-1---introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
- [Day-2 - Introduction to ABI and basic verification flow](#day-2---introduction-to-abi-and-basic-verification-flow)
- [Day-3 - Digital Logic with TL-Verilog and Makerchip](#day-3---digital-logic-with-tv-verilog-and-makerchip)
- [Day-4 - Basic RISC-V CPU micro-architecture](#day-4---basic-risc-v-cpu-micro-architecture)
- [Day-5 - Complete Pipelined RISC-V CPU micro-architecture](#day-5---complete-pipelined-risc-v-cpu-micro-architecture)
- [References](#references)


### Day-1 - Introduction to RISC-V ISA and GNU compiler toolchain  
<details>
<summary>
Introduction to RISC-V basic keywords 
</summary>

- **RISC-V**: It is the medium of communicating with the computer. It is also known as Instruction Set Architecture(ISA). If we want to run a C-program on a computer, it is first converted into an assembly level language and further this is ocnverted into machine language i.e; binary which is the only language understood by the computer. We reuire another interface between the RISC-V and the computer, which is Hardware description language.  

- Inorder to run a software on a system/hardware, we will make use of a system called System Software. This system software mainlt consists of 3 blocks: OS, Compiler and Assembler. 
</details>

<details>
<summary>
Labwork for RISC-V software toolchain  
</summary>

 1. Let us start with a simple c-program with name 'sum1ton.c'. The code is as follows:
 ```
 #include <stdio.h>
 int main() {
	int i, sum = 0, n = 100;
	for (i=1; i <= n; ++i) {
	sum += i;
	}
	printf("Sum of numbers from 1 to %d is %d\n", n, sum);
	return 0;
}
```
The implementation  and the output of the above code is shown here: <img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/74e7cf59-fbe2-4be7-92ae-5f837592adb2">  

2. Let us now compile the sum1ton.c file using risc-v simulator using following codes:
```
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
$ riscv64-unknown-elf-objdump -d sum1ton.o | less
```
The main content assembly code is as follows:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c7709eef-f164-40f3-84b2-d99b735199dd">  

Let us run the same using a slightly different command and observe the output:
```
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
$ riscv64-unknown-elf-objdump -d sum1ton.o | less
```
<img  width="500"  alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6ebbf3f7-212f-4eaa-948d-16b6f22c37a3">  

3. Now, let us learn how to get the output to display even when we run code using the RISC-V compiler:
```
spike pk sum1ton.o
```

<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/f1aa123e-76f0-4a33-a344-f8198a695828">    


Let us now debug the above:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/29383d37-177e-4893-a17d-2b9fc611fe97">  
 
</details>

<details>
<summary>
Integer number representation
</summary>
</details>

### Day-2 - Introduction to ABI and basic verification flow  

<details>
<summary>
 Application Binary interface (ABI) 
</summary>
</details>

<details>
<summary>
Lab work using ABI function calls
</summary>
</details>

<details>
<summary>
Basic verification flow using iverilog 
</summary>
</details>


### Day-3 - Digital Logic with TL-Verilog and Makerchip  

<details>
<summary>
Combinational logic in TL-Verilog using Makerchip 
</summary>
</details>

<details>
<summary>
Sequential logic 
</summary>
</details>

<details>
<summary>
Pipelined logic 
</summary>
</details>

<details>
<summary>
Validity 
</summary>
</details>

<details>
<summary>
Wrap-up 
</summary>
</details>

### Day-4 - Basic RISC-V CPU micro-architecture

<details>
<summary>
Introduction to Simple RISC-V Micro-architecture 
</summary>
</details>

<details>
<summary>
Fetch and decode 
</summary>
</details>

<details>
<summary>
RISC-V control logic
</summary>
</details>

### Day-5 - Complete Pipelined RISC-V CPU micro-architecture  

<details>
<summary>
Pipelining the CPU 
</summary>
</details>

<details>
<summary>
Solutions to Pipeline Hazards 
</summary>
</details>

<details>
<summary>
Load/Store Instructions and Completing RISC-V CPU
</summary>
</details> 


### References  
1. https://www.vsdiat.com/course_content?uniqueid=20220802023852  

