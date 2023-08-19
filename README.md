# RISC-V

### TABLE OF CONTENTS
- [Day-1 - Introduction to RISC-V ISA and GNU compiler toolchain](#day-1---introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
- [Day-2 - Introduction to ABI and basic verification flow](#day-2---introduction-to-abi-and-basic-verification-flow)
- [Day-3 - Digital Logic with TL-Verilog and Makerchip](#day-3---digital-logic-with-tl-verilog-and-makerchip)
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
The implementation  and the output of the above code is shown here: <img width="520" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/74e7cf59-fbe2-4be7-92ae-5f837592adb2">  

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

1. As computer can only understand binary number, we need to learn how to convert the decimal to binary and viceversa and understand it.

- In RISC-V architecture, we call the 64-bit binary number as **"Double word"**, and a 32-bit number as a **"word"**.
- A group of 8 bits is termed as **"Byte"**.
<p align="center">
<img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/de2a5767-b8f4-43e2-8c7f-2ef20c06a6ba">
</p>

- The total unsigned numbers we can form using n-bits is given as  : **2^(n) - 1**.
- We use 2's complement representation to represent the negative numbers.
- For signed representation, the MSB bit indicated the sign of the number. If MSB=0, it is a positive number and MSB=1 indicates a negative number.
<p align="center">
<img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/376bd36b-ab55-44d5-bcb5-56c41c6d3657">
</p>

- In signed representation of binary numbers, the range of positive numbers we can represent using n-bits is: **0 to (2^(n-1) - 1)** and the range of negative numbers is: **-1 to -2^(n-1)**.

2. Let us do a lab exercise based on the signed and unsigned binary numbers:

- The following code is to rpresent the highest binary number in unsigned representation:
```
#include<stdio.h>
#include<math.h>

int main() {
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	printf("highest number represented by usigned long long int is %llu\n", max);
	return 0;
}
```
The output is as follows: <img width="600" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/3fd06edc-7995-44e5-8ad4-d77048ffabfc">  

Now, if we modify the code to give the representationof a higher bit number, the output still remains the same because the data type unsigned long long int only supports 64-bits.
Let us edit the data type as long long int and rerun the code:
```
#include<stdio.h>
#include<math.h>

int main() {
	long long int max = (long long int) (pow(2,10) * -1);
	printf("highest number represented by long long int is %lld\n", max);
	return 0;
}
```
The output is as follows: <img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/f5f710c6-3561-44d2-a3a4-84501a53a98d">  

Create a new file signedHighest.c with the following code in it:
```
#include<stdio.h>
#include<math.h>

int main() {
	long long int max = (int) (pow(2,63) -1);
	long long int min = (int) (pow(2,63) * -1);
	printf("highest number represented by long long int is %lld\n", max);
	printf("lowest number represented by long long int is %lld\n", min);
	return 0;
}
```
The output now is: <img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/72b68511-f991-40df-9b48-c9d5978eabf7">  

Here, the outut getting displayed is wrong. Inorder to get the correct output, modify the code as following:
```
#include<stdio.h>
#include<math.h>

int main() {
	long long int max = (long long int) (pow(2,63) -1);
	long long int min = (long long int) (pow(2,63) * -1);
	printf("highest number represented by long long int is %lld\n", max);
	printf("lowest number represented by long long int is %lld\n", min);
	return 0;
}
```
Now, we ge the desired results: <img width="550" alt="Screenshot from 2023-08-19 18-01-41" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c70cabae-f55d-45a3-a49f-2382f7a4544e">  


</details>

### Day-2 - Introduction to ABI and basic verification flow  

<details>
<summary>
 Application Binary interface (ABI) 
</summary>

- For a computer, the Interface for the users means the appearance and functionality of the system. It does not bother about the implementation procedure and the processors used.
- Inorder for an application to run on the hardware there are many intermediate stages that the program has to undergo. The below example depicts the stages involved for an e-mail application to run on a hardware:
<p align="center">
<img width="600" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/463092a4-9bfb-4101-926d-65693a32ba88">
</p>  

**Application Binary Interface**:
- It is a mode through which the application programmer can access the contents of hardware resources of the processor. The access of porcessor is done via registers.
- In RISC-V specification, we have 32 registers whose width is defined by the keyword "XLEN". It is XLEN-32 bit for Rv32 and XLEN-64 for Rv64.
- For RV64, the data can either be loaded to registers directly or we can first load tha data into memory which holds 8-bits in each memory address and then transfer it to the registers.
- All the instructions in RISC-V is of 32-bits.
- **ld**(load doubleword) is a command to load the contents of memory into register. 
- **add** is used to add the contents of the registers/memory. 
- **sd**(store doubleword) is used to store the contents of register back to the memory.

The summary of the above instructions is shown below:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/e3325e6d-f928-48f0-83f3-061bdcfa4423">  
</p>

- The instructions which operate only on registers are called **R-type** instructions.
- The instructions which operate on immediate as well as registers are called **I-type** instructions.
- The instructions which operate only on source registers and immediate are called **S-type** instructions.

- We will use 5-bits to represent the registers which is why we have only 32 registers in RISC-V. The registers fuctions are as follows:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/2a876d28-f15e-4fff-96bf-8bd20139dfa4">  
</p>

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

