# Processor and Pipeline Fundamentals

## The Basics MIPS Processor

![[Pasted image 20261009105116.png]]

Stands for *Microprocessor without Interlocked Pipeline Stages, its essentially a model of Harvard Architecture.*

1. **Program Counter (PC)**
	1. Holds the address of the next instruction to execute.
	2. Sends the address to the instruciton memory. The dedicated top **Adder (add)** 
2. **Instruction Memory** - Stores actual instructions code. Takes address from PC and pulls out the raw 32-bit instruction that needs to be executed.
3. **Registers** - 32 extremely fast memory slots used in the execution of instructions.
4. 

