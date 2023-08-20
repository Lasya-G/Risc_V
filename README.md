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

Let us use the same example of sum of 'n' numbers in c-language but using a different approach.The algorithm used to re-write the code is shown here:   
<p align="center">
<img width="450" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/899c27a7-f804-4f87-bcb8-99574a8432e6">  
</p>
  
Create a c file with name 1to9_custom.c and write the following code in it:
```
#include <stdio.h>

extern int load(int x, int y);

int main() {
	int result=0;
	int count = 0;
	result = load(0x0, count+1);.global load
	printf("Sum of number 1 to %d is %d\n", count,result);
}
```
Create another file named load.s and dump the following code into it:
```
.section .text
.global load
.type load, @function

load:
	add	a4,a0,zero
	add	a2,a0,a1
	add	a3,a0,zero
loop:
	add	a4,a3,a4
	addi	a3,a3,1
	blt	a3,a2,loop
	add	a0,a4,zero
	ret
```
Let us run the above codes using spike compiler and observe:
<img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/cb5e8ad4-7dd5-4f9f-a84a-b86ca9311663">  

Use the following command to view the assembly code generated:
```
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

The main program is as follows: <img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/41c2e680-f090-4d22-a9ff-938941f09df5">  

</details>

<details>
<summary>
Basic verification flow using iverilog 
</summary>

We will follow the following procedure in this lab session:  
<p align="center">
<img width="450" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/8e983904-1e1d-4dde-8280-1900454007ab">  
</p>

Use following commands to the riscv cpu program code:
```
vim 1to9_custom.c
./rv32im.sh 
```
The following output is observed: <img width="550" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/7f676179-c142-4b00-8348-58e5841a770d">  

The following are the hex files:  

- firmaware.hex:
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a32ba685-cd89-452f-8c02-f11fe04708e3">

- firmware32.hex:
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/fab6f05e-98ee-44d7-8525-f20a92f3f36f">  

</details>


### Day-3 - Digital Logic with TL-Verilog and Makerchip  

<details>
<summary>
Combinational logic in TL-Verilog using Makerchip 
</summary>

The logic gates are the fundamental building blocks of digital circuits:
<p align="center">
<img width="450" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/66563732-9bee-430f-b6e5-76b80fa58d25"> 
</p>

These fundamnetal blocks are connected together to form the most complex circuits.
Consider the following full adder circuit:
<p align="center">
<img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/166fc608-5b6f-4c06-921f-d5c2d7525d84"> 
</p>

Let us now use this full adder as a basic block to build complex circuitslike an n-bit adder:
<p align="center">
<img width="450" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/07216113-b9c2-4cc1-83b6-306e98f14fde">  
</p>

Some basic boolean operators are listed below:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6ff96384-838b-4fca-949f-ba16f6d56895">
</p>

Let us now take a look at the **MULTIPLEXER(MUX)** block and it's function:
<p align="center">
<img width="300" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/eecf29a3-c0a4-4144-8c84-7fe04eb22857">  

We use the following syntax to express the mux in the verilog:
```
assign f = s ? x1 : x2;
```

Now, take a look at the Chaining Ternary Operator(4-bit mux):  
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/bccff903-7dcc-40b3-92ec-f8b87f479505">   
</p>

Now, let's begin with the **Makerchip**:  
- Go to makerchip.com and launch Makerchip IDE.
- Go to Learn, click on Examples and select FPGA multipler.
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/23dbaa4a-8862-4742-8a17-ea900db30a18">
</p>


**LAB-1** - Makerchip Platform: 
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/e545e873-c529-4f5b-b51f-4294f4342ea0">  
</p>

**LAB-2** - Combinational Logic:   
1. INVERTER:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/7daabdf7-72ff-4946-9824-e9686348ead3">
</p>

2. AND GATE:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/9e97649e-4ba5-4372-929f-6189ac43fef2">
</p>  

**LAB-3** - Vectors:    
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6f9d5e55-a01c-4787-b061-c48759716055">
</p>  

**LAB-4** - Mux:  
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/d528baab-8dbe-4f5b-bf3b-cb03f4981c73">
</p>  

<p align="center">
<img width="400" alt="Screenshot from 2023-08-19 23-34-42" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5e99d722-c352-4e0e-9fa5-ab3bdf584f07">
</p>  

**LAB-5** - Coombinational calculator:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/f5fff139-b79d-45d3-888d-1ba3aad1b629">
</p>  

</details>

<details>
<summary>
Sequential logic 
</summary>

- Sequential logic introduces a clock in the circuit. <img width = "500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/de6626ce-063c-47e4-a068-a4486bd316e1">

A D-Flipflop is responsible for allowing the values to propagate upon the clock edge. <img width="300" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6e1f74bf-09ab-4a5d-accc-6f64780f5b97">  

The reset signal is used to get all the flipflops into a known state.
The whole sequential circuit can be viewed as a big state machine: <img width="300" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/58fcf2b7-a5a0-4a97-a85c-a338df4b9ebd">  
Upon the clock the combinational circuit does a new computation with the updated state.

**LAB** - Fibonacci Series:
<p align="center"">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/34fec31f-d0fd-40a9-a9a6-225e488ecb4e">
</p>

**LAB** - Counter:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/43adef2f-a0a8-40e7-8818-ff28cc5b79f4">
</p>

The values in verilog are represented as:  
<img width="300" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/d09876ff-1b25-45b4-b47c-5479cb474cae">  

**LAB** - Sequential Calculator:  
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/55e8dacd-184f-4755-8ab5-3b788d922705"> 
</p>

</details>

<details>
<summary>
Pipelined logic 
</summary>

Let us glimpse at the Pythagoran's Theorem and compute it in hardware.  
Pythagoran's Theorem: <img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/1b824835-a616-40ce-90d8-b1dbced71b20">  
Let us compute the pythagoran's theorem over 3 cycles.  
- Cycle1: Squaring on the sides a and b.
- Cycle2: Adding the sqyared vales of a and b.
- Cycle3: Finding the square root value of the sum.
<p align="center">
<img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/13066f2e-9192-491f-86e7-ac58eca11241">
</p>  
The timing abstract of the above is:   
<img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/7af761d8-9158-436d-a189-dbda56eabf76">  

The makerchip implementation of  Pythagoran's theorem is given below:  
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/eb075aed-7678-431e-a4db-e413257692f5">
</p>

- **Code reduction** is the most advanatageous property of the TL-Verilog when compared to System Verilog. We can use the above code to compare this:
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/b2428652-e85f-4974-9237-1b32a41741d0">

- The **Retiming** property in TL-Verilog is very easy and safe to implement whereas in SystemVerilog, it is very bug-prone.
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/9ded0b8f-8148-4637-a571-023f02398994">

- The pipelinig also allows us to run the clock at a high frequency. Regardless of the way we structure our logic, we will be able to produce new set of inputs on every clock edge. As a result, we get high throughput for our circuit.  
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/ea35bbfa-e00c-45e2-bc64-931dac742ad6">

**Syntax** in TL-Verilog:
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/b7cb7914-293e-40b3-9557-6ba14afac885">  

Let us now implement the Fibonacci Series in a pipeline:
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/87f04dc1-bd24-48b5-a08c-230709f4bb95">  
</p>

Implementation of pipeline through TL-Verilog:  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/0cb545e1-6049-442e-aa73-0d91a2bd6b79">    

In the above implenation, we can observe the errors in the pipeline:
<img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c086e074-7b95-45cf-8ba7-19742fee1696">  

**LAB1** - Counter and Calculator in pipeline:  

The strcture of the pipeline is as follows:  

<img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/d98cfdde-b748-4a01-9b4a-44a28655dc53">  

The makerchip implementation output:  

<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c3d15a76-2dfa-46d4-8c04-5acda191ed96">
</p>

**LAB-2** - Cycle Calculator:  
The structure of the pipeline: <img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/1f6e570f-9695-48f8-be21-3ba0d31b5bd9">   



The makerchip implementation output:   
<p align="center">
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/67da9907-1ed6-40d2-99b1-d3ab20c9cf41">
</p>  


</details>

<details>
<summary>
Validity 
</summary>  

**Validity** is a notion for when the values or the signals are meaningful.  
<img width="250" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/54a44194-cae1-4214-947c-f4a5988b3a0b">  

Let us implement the Pythagoran's theorem with validity:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/10857531-6bc5-401e-88a9-6bea107e42a4">
</p>  


**Clock Gating** is a power-saving property.  
<img width="450" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/4656e7b6-a6f9-46f0-8c2b-fd13622e0bd3">    


**LAB** - Distance Accumulator with Pythagoran's theorem:  
The pipelines structure is : <img width="350" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/bdf54ca4-13e0-440d-96da-d5ad891af409">  

The implementation output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/06976bd7-4b24-4fe2-a195-5e55387db38c">
</p>  


**LAB** - Cycle calculator with Validity:    

The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/66da50d5-3b5b-41bd-85c4-c8b5692870fb)">  

The makerchip implementation output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5116f2f9-7164-4de4-830d-fd029e8843bf">
</p>  

**LAB** - Calculator with single value memory:  

The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/8c4c9737-f7dc-4c54-ace7-38cd7d54b284">  

The makerchip implementation:  

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5097273f-0293-4b3a-a860-7feaef125315">
</p>


 
</details>

<details>
<summary>
Wrap-up 
</summary>

**Hierarchy**    

**LAB** - conway's game of life:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a920aa43-4c38-4d16-a23d-56c03d88fef2">
</p>   

**LAB** - Pythagoran's theorem:  

The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/586fe3a1-c522-4896-9982-4ca4aa799346">  

The makerchip output:  

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c61529cd-6c60-485b-90e5-2c3d6a52c955">
</p>

</details>

### Day-4 - Basic RISC-V CPU micro-architecture

<details>
<summary>
Introduction to Simple RISC-V Micro-architecture 
</summary>

The micro architecture for the RISC-V implementation is shown here:  
<img width="400" align="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a6b48328-562a-4bf4-b061-1ee750ea6a8f">  
- The **Program counter** is a pointer to the instruction memory as to which instrcution must be executed next.
- The **Decoder** interprets the instruction and send signals regarding the action of the processor and the location of data. The decoder also sends incremented by 1 value to the PC, instructing it to move to the next instruction.
- **Register Files** implements the read and write operations on the data/memory.
- **ALU** computes the arithmetic operations and write the result back to the register file.

</details>

<details>
<summary>
Fetch and decode 
</summary>

The implementation plan of RISC-V CPU Core:  
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/0349e97f-0823-4c77-9be4-8f0fb13feb43">  

**LAB - PC**:  

The implementation pipeline: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a0ed2e11-7d97-4f2e-b9c7-844c511f77c5">  

The makerchip output:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/fcdb6430-244e-4298-a07e-4b07948efd91">
</p>

**LAB - FETCH**

The pipeline structure(part-1): <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a48ff16d-e3d3-4745-8e35-a09002283026"> 

The pipeline structure(part-2): <img width="250" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/75dfe66b-b3c7-40ad-96ba-f4f17e88ed08">    


The makerchip implementation output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6bd09d0a-5372-4c5f-a7f1-04a40dfa3764">
</p>

**LAB - INSTRUCTION TYPE DECODE**  


The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5b1eec5d-61a0-43fe-992a-c42b2e2f96bf"> 


The makerchip output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/32e623de-acfc-45e5-9459-b41323abec30">
</p>


**LAB - INSTRUCTION IMMEDIATE DECODE**

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5be86415-fb71-435f-ad36-c1d215643384">  

The implementation output:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/3f0c95e6-6fa2-47a7-a580-3a0c3fee30aa">
</p>  


**LAB - INSTRUCTION DECODE**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/50dd4c65-c605-43ab-b420-6a10d0d917e8">

The implementation output:  
<p align="center">
<img wudth="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/c65173a1-6f61-42bf-a0df-eef102cecd0a">
</p>

**LAB - INSTRUCTION FIELD DECODE**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/fc794260-873d-4817-a5fc-986d1979472b">

The implementation output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/2cac17be-93d1-4f17-b305-49da3a5ce7b9">
</p>

**LAB - INSTRUCTION DECODE_2**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/7d1c0881-270a-4664-9496-0f8b7cd3d7b0">  


The implementation output:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/78350b64-dcc6-4c74-808d-e304876fa056">
</p>

</details>

<details>
<summary>
RISC-V control logic
</summary>

**LAB - REGISTER FILE READ_1**  

The pipeline structure is as follows:  <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/eca9ac67-bee2-4567-9b4c-9f51d7947595">    

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/8835d312-fde2-47bb-93be-f6c6c9ffd928">  


The makerchip implementation output:  
<p align="center">
<img width="500" alt="image" src="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/ab48af08-7d7b-4877-8aef-b9a06bf5c5db">
</p>  

**LAB - REGISTER FILE READ_2**  

The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/8252ff3d-1b4a-48b1-a408-af2fe0de9a1d">  


The makerchip implementation output:  
<p align="center"> 
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/19837024-1468-414d-8ff6-0bcd1d60e59a">
</p>

**LAB - ALU**  

The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/9e8e5f77-8ab5-4a76-8378-9b646307e80d">  

The implementation output:  
<p align="center"> 
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/083a9454-1832-4621-a734-24df5dbc2850">
</p>



**LAB - REGISTER FILE WRITE**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a04e26da-f6ac-4830-af4e-e2086983597f">  

The makerchip implementation output:  
<p align="center"> 
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/57227a1a-9079-410a-a74e-f38098ae6931">
</p>

**ARRAYS**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/d99f90b2-c28f-4f96-b819-574403e3024c">  

The detailed implementation of Register files is given below:  
<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5252e9c1-e29a-4e6b-8e52-c495c35d8627">  


**LAB - BRANCHES**

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/4ce5b4ad-9c09-430e-bcf1-6b39dfea28dc">

The implementation output:  
<p align="center">
<img width="500" alt="!mage" src="https://github.com/Lasya-G/Risc_V/assets/140998582/e874bb8b-ea31-4ca6-b185-9edf13a1bdf4">
</p>

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/e9c60983-9303-463f-8085-6e87b30eab0e">  

The output is as follows:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/7dab870a-cec7-43f3-84fb-b9c2e9221987">  

**LAB - TESTBENCH**  

The makerchip implementation output:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/e0013498-a534-4624-aeee-91e93cacd4d9">
</p>

   
</details>

### Day-5 - Complete Pipelined RISC-V CPU micro-architecture  

<details>
<summary>
Pipelining the CPU 
</summary>

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/1c23c767-e291-430f-ab0b-b85e6924f015">  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/bef861e4-e993-42d9-9c0e-d05f45bdc4fb">  

**LAB - 3-CYCLE VALID SIGNAL**    

The implementation output is:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/4e094372-0f25-4c77-b7c1-906d3b04ee5f">   
</p>  


**LAB - CYCLE RISC-V**  

<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/4b65e2d2-8dba-4484-8bf7-a43a7f120a71">  
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/89cd76b4-80b1-4901-bc56-f460c01beec9">  



The implementation output is:  

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5eb309b3-0acb-47da-aa94-0e03d24f3205">
</p>  


</details>

<details>
<summary>
Solutions to Pipeline Hazards 
</summary>

**REGISTER FILE BYPASS**

<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/50d9c9b0-05bc-4a5a-976e-c253deddaa59">  

The pipeline structure:<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5cd0ee1c-691a-4eef-be9f-a9c79873f96d">  

The implementation output is as shown below:  
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/d15c47be-6335-4db8-9689-429d927a1ca5">  
</p>

**LAB - BRANCHES** 

The pipeline structure:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/5840bbcc-26e1-4132-9cfa-2a28dcccdfa4">

The implementation output:

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/6f4a4895-03ea-47fd-8a19-ae23269ec885">  
</p>

**LAB - ALU**   

The makerchip implementation results are:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/96477a58-80a0-43d8-9716-11be0385e3e9">  
</p>


</details>

<details>
<summary>
Load/Store Instructions and Completing RISC-V CPU
</summary> 

**LOAD**  

<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/0e33a0c9-5854-41af-819f-468a768fadd1">  
 
The pipeline structure: <img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/700c2548-85b4-48c1-b16a-2324f20f615c">  

The implementation output:
<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/a5bbd00d-df08-4294-bcfe-3f1157b88b0a">
</p>

**LOAD/STORE**  

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/1eded761-772a-46e3-9a80-584da3f4afb5">

The implementation output:  

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/4106c86c-15c4-4543-8a40-67fa9e28d4bf">
</p>

**JUMPS**

<img width="400" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/61ab2fae-cb27-44dc-a89a-82f242d88c6a">   


The makerchip output:  

<p align="center">
<img width="500" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/8be22a61-dbad-4f2d-8521-03a787b7266f">
</p>

**RISC-V CORE CPU - FINAL IMPLEMENTATION**  

<p align="center">
<img width="600" alt="image" src="https://github.com/Lasya-G/Risc_V/assets/140998582/431a593f-7b90-4825-89dc-bf85374f9090">
</p>












</details> 


### References  
1. https://www.vsdiat.com
2. https://github.com/stevehoover/RISC-V_MYTH_Workshop
3. http://makerchip.com/sandbox/

