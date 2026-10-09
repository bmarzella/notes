# Processor and Pipeline Fundamentals

## The Basics MIPS Processor

![[Pasted image 20261009105116.png]]

> *Stands for **Microprocessor without Interlocked Pipeline Stages**, it's essentially a model of Harvard Architecture.*

1. **Program Counter (PC)**
	1. Holds the address of the next instruction to execute.
	2. Sends the address to the instruction memory. The dedicated top **Adder (add)** increments the address by +4 (length of a MIPS instruction) to prepare for the next cycle, unless a jump or branch occurs. In this case, the **second adder overwrites the PC with a targeted destination address instead.**
2. **Instruction Memory**
	1. Read- only memory that holds compiled machine code, the actual instructions, of the program.
	2. Accepts memory address from PC and outputs the 32-bit instruction word, splitting it up to send the appropriate components.
3. **Registers File**
	1. 32 extremely fast memory slots used in the execution of instructions.
	2. Instruction word selects and opens the required source registers (*Reg.#*) and the register file outputs their values (*Data*) to the ALU.
4. **Arithmetic and Logic Unit (ALU)**
	1. Logic circuit responsible for performing arithmetic (*ADD, SUB*) and logical operations.
	2. Executes required operation on data values pulled from registers. Output determines either a calculated value (sent to the register file) or a target memory address (sent to the data memory).
5. **Data Memory**
	1. Interface with RAM, holds long -term runtime variables and data structures.
	2. Uses ALU output as a memory address (*Addr*) to either read a value from or write data to memory. Data read from memory is sent back to be saved into the Register File.

## Logic Design Convention

### Combinations vs Sequential Logic

![[Pasted image 20261009112048.png]]

- **Combinational logic** - Output depends *only* on current inputs. It has no memory (*no Registers, Flip-Flops, RAM etc.*)
- **Sequential Logic** - Output depends on current inputs *and* past state. Contains memory.
- **Which is needed to build an ALU**