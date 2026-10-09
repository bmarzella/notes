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

1. **Combinational vs Sequential**
	- **Combinational logic** - Output depends *only* on current inputs. It has no memory (*no Registers, Flip-Flops, RAM etc.*)
	- **Sequential Logic** - Output depends on current inputs *and* past state. Contains memory.
2. **Control vs Data Signals**
	- **Data Signals** - Actual numbers being processed by or passed around within the processor. 
		- *32-bit numbers read from registers, immediate values or memory read data, for example.*
	- **Control Signals** - Command lines that determine what happens to the data.
		- *Setting a MUX line to choose between two inputs, enabling/disabling register writes, telling the ALU which operations to write, for example.*
3. **Why do we need a clock?**
	- Combinational circuits take a small, non -zero amount of time for electrical signals to settle (*propagation delay*).
	- A clock ensures that inputs hold still long enough to perform calculations, and that state elements (like registers and memory) only capture new values at precise, predictable intervals (*the clock edge*).

## Building a Single Cycle MIPS Datapath
### The Three Core Blocks

![[Pasted image 20261009113025.png]]

1. **Instruction Fetch & PC**
	- Fetches the current instruction and increments the PC.
2. **Registers & ALU Execution**
	- Reads source registers (input values) and performs arithmetic/logical operations.
3. **Load and Store/Memory Access**
	- Sign-extends 16-bit intermediate values to 32 bits and handles reads/writes to memory.

- **The Sign-Extend Unit**
	- Takes a 16-bit intermediate field (from instructions like `lw`, `sq` or `addi`) and expands it to 32 bits by replicating the sign bit, allowing the ALU to operate on it.

### Connecting the Blocks Together

![[Pasted image 20261009113509.png]]

We connect the three blocks into a full **Single-cycle Datapath** by adding *Multiplexers* and *Branch Logic*.

#### Multiplexers (Muxes)
##### What is it?
A digital switch with multiple data inputs, one data output, and a control line. Depending on the control signal (0 or 1), it shows which input gets passed to the output.
##### Why do we add it?
A single component such as the ALU or Register file often needs to receive data from different sources depending on instruction type. **The Mux directs the traffic so data from the right source gets through.**
##### The diagram
1. *`ALUSrc` Mux* -  Decides whether the ALU's second input comes from a Register (for `add`/`sub`) or an immediate value (`lw`/`sw`/`addi`).
2. *`MemToReg` Mux* - Chooses whether data written back to a register comes from the ALU result (for arithmetic) or data memory (for `lw`).
3. *`PCSrc` Mux*  - Chooses whether the next instruction address is standard `PC + 4` or a branch target address.

#### Branch Logic
##### What is it?
Hardware dedicated to executing conditional jump instructions (`beq` - branch if equal, for example). Consists of an **Adder, a Shift Left 2 unit and an AND gate** connected to the ALU's `zero` signal.
##### Why do we add it?
Standard execution increments the PC by 4 per cycle, this allows us to implement branch logic.

##### How does it work?
1. **Address Calculation** - 16-bit offset from the instruction is sign-extended, multiplied by 4 (using `shift left 2`), and added to `PC + 4` using the dedicated Branch Adder.
2. **Condition Checking** - The main ALU subtracts the two registers being compared. If the result is zero, the ALU sets its `Zero` single to $1$ (meaning the values were equal).
3. **Decision** - If both the branch control signal and `Zero` signals are $1$, `PCSr` switches the top Mu to update the PC with the branch target address instead of `PC + 4`.

### The ALU Control Unit

![[Pasted image 20261009115705.png]]

#### Two Level Decoding
Instead of a massive control unit that decodes every combination at once, MIPS uses a two-level decoding system:
1. **Main Control Unit** reads the OpCode (bits 31-26) and produces a 2-bit `ALUOp` signal.
2. The **ALU Control Unit** takes `ALUOp` + `Funct Field` (bits 5-0) to output the final 4-bit `ALU Control Input`.

> *The Main Control unit decides whether the instruction is R, I or J-type. If it's an R-type, the ALU Control Unit inspects the `Funct` field to pick the exact operation, since all R-type OpCodes are identical (`000000`). For I-type or J-type, the Main Control Unit's `ALUOp` signal directly tells the ALU Control Unit what to do (e.g., force an add for `lw`/`sw`), ignoring the `Funct` field.*

#### Instruction Classes
##### R-Type
*Register type. An instruction that operates entirely between registers.*

![[Pasted image 20261009153719.png]]

- **OpCode** - Identifies the instruction as R-type. Always 0 (`000000` in binary)
- `rs` - First source register.
- `rt` - Second source register.
- `rd` - Destination register (where result is saved).
- `shamt` - Shift amount (used for shift operations, otherwise 0).
- `funct` - Function field, tells the ALU Control unit the exact operation.

##### I-Type
*Immediate type. An instruction that involved two registers and a hardcoded, 16-bit constant or memory offset.*
###### Load/Store
*Moving data between memory and registers.*

![[Pasted image 20261009154358.png]]

- **OpCode** - `35` for `lw` (load word) or `43` for `sq` (store word).
- `rs` - Base address register.
- `rt` - Destination register for `lw`, or source register for `sw`.
- `address`/`offset` - 16-bit immediate value added to `rs` to get the target memory address.

###### Branch
*Comparing values to decide whether to branch.*

![[Pasted image 20261009154703.png]]

- **OpCode** - `4` for `beq`, (branch on equal).
- `rs` - First register to compare.
- `rt` - Second register to compare.
- `address`/`offset` - 16-bit relative branch offset, shifted left by 2 and added to `PC + 4`.


### Control Signals

![[Pasted image 20261009154923.png]]

1. 