# Processor and Pipeline Fundamentals

## The Basics MIPS Processor

![[Pasted image 20261009105116.png]]

Stands for *Microprocessor without Interlocked Pipeline Stages, its essentially a model of Harvard Architecture.*

1. **Program Counter (PC)**
	1. Holds the address of the next instruction to execute.
	2. Sends the address to the instruction memory. The dedicated top **Adder (add)** increments the address by +4 (length of a MIPS instruction) to prepare for the next cycle, unless a jump or branch occurs. In this case, the **second adder overwrites the PC with a targeted destination address instead.**
2. **Instruction Memory**
	1. Read- only memory that holds compiled machine code, the actual instructions, of the program.
	2. Accepts memory address for PC and outputs the 32-bit instruction word, splitting it up to [word weirdly don't likeit]
3. **Registers** 
	1. 32 extremely fast memory slots used in the execution of instructions.
	2. Inputs of the instruction word select the source registers (*Reg.#*) and the regiter file outputs the corresponding numberical data (*Data*) to the ALU.
4. 

