[Slides](https://blackboard.durham.ac.uk/ultra/courses/_73392_1/file/_3769558_1?courseId=_73392_1)
# Pegasus

![[Pasted image 20261009132553.png]]

- Exploit in WhatsApp allowed RCE which was leveraged for spyware.
- Marketed for law enforcement but was abused

# Buffer Overflow Recap

![[Pasted image 20261009132709.png]]

- C in fundamentally unsafe in the way it handles memory.
- For example, we can ride past the end off the buffer in this code to access data that shouldn't be.

Let's look at what the stack looks like, executing this code normally:
## Stack on 64-bit x86
### Registers
There are three registers used for working with the stack:
1. **Register Instruction Pointer** (`RIP`)
	- Points to the code instruction currently being executed. 
2. **Register Stack Pointer** (`RSP`)
	- Points to the top of the stack frame in memory.
3. **Register Base Pointer** (`RBP`)
	- Points to the base of the active stack frame in memory.

### Steps

![[Pasted image 20261009142436.png|220]]  ![[Pasted image 20261009142751.png|418]]
#### 1. `push str`
`str`, the argument passed into the function at `func(attacker_controlled_string)`, is pushed onto the stack and the RIP is incremented to point to the next instruction.

![[Pasted image 20261009143216.png|292]]![[Pasted image 20261009142815.png|353]]  
#### 2. func`
We call the function `func` and push the address of the instruction to be completed after `call func` in `main` to the stack, (the **return address**). When `func` is completed, we know where to go back to. RIP is updated to point to the start of `func`.

*Note that `main+x+2` would make more sense in the slide.*
![[Pasted image 20261009143442.png|304]]![[Pasted image 20261009142929.png|335]]

 We then push the old `RBP` to the stack, so when we return to `main` we know where in the stack to continue from, and update it to point to the current top, which will be the bottom of `func`'s stack frame. I'll stop mentioning RIP until it is updated, but know it is incrementing through `func`'s instructions.
 
![[Pasted image 20261009144242.png|469]]

We reserve a **16-byte** area on the stack for `char buffer[16]`. When `strcpy` runs, it copies `"Hello, World\0"` (13 bytes) into `buffer`. This leaves 3 bytes of unused space inside the buffer, so no overflow occurs yet.

![[Pasted image 20261009144637.png|226]]

#### 3. Cleaning Up
Once we finish with `func`, we need to clean up the stack to continue running main.
1. Discard the buffer: We move `RSP` back down to `RBP`, which instantly discards buffer\[16].
2. Restore `RBP`: We pop *Old RBP* off the stack into the `RBP` register so it points back to main's base frame, leaving` Return addr` at the top of the stack.
3. Return to main: The `ret` instruction pops `Return addr` off the stack into `RIP`, pointing to the next instruction in main.

![[Pasted image 20261009145330.png|311]]![[Pasted image 20261009145337.png|326]]

# How do we exploit this?
#### 1. The Initial Stack
The stack is set up normally, and we begin with a buffer on top ready to take the attacker supplied argument.

![[Pasted image 20261009150348.png|198]]

#### 2. Malicious Input
The attacker has supplied an malicious input, which consists of three parts:
1. **16-byte Shellcode**
	- This fills `buffer[16]`. 
	- Anything subsequent will "overflow" over into contiguous memory space.
2. **Fake `RBP` Address**
	- This overwrites the `Old RBP`.
3. **Target Return Address**
	- A fake return address supplied to overwrite the real `Return addr`.
	
![[Pasted image 20261009150408.png|218]]

#### 3. The Exploit
When `func` finishes and attempts to clean up:
1. **`leave` executes:**
    - `RSP` moves to `RBP` (discarding the shellcode in `buffer[16]`).    
    - The CPU pops the **Fake RBP Address** off the stack into the `RBP` register.
2. **`ret` executes:**
    - The CPU pops the **Target Return Address** off the stack and loads it directly into **`RIP`**.
- **Execution Hijack:**
    - Because `RIP` now holds the attacker's **Target Return Address** (which usually points right back to `buffer[16]` where the shellcode is sitting), the CPU jumps directly into `buffer[16]` and begins executing the attacker's **16-byte Shellcode**.

![[Pasted image 20261009133547.png]]

# Mitigation
The first mitigation was to **Mark the stack as non-executable**. We can jump to wherever we want, but we cant execute code there.

![[Pasted image 20261009133746.png]]

## Return Oriented Programming

***What if we jump to existing code?***

Instead of executing our own code, we could jump to existing code, say, *access granted.*

We supply arbitrary values in place of the shellcode and fake `RBP`, and point the return address to somewhere we want. The following, for example, gains a shell:

![[Pasted image 20261009151023.png]]
![[Pasted image 20261009151036.png|634]]

# A Note on Calling Conventions in Linux

- On modern 64-bit Linux systems, functions don't use the stack for arguments if they don't have to, instead using CPU registers as they are much faster.
- If a function has more than 6 arguments (`RCX` is missing from the slide), then any subsequent arguments are pushed onto the stack. The function sets up `RSP` and `RBP` as we saw.
- When a function finishes and wants to return a value, it puts 

![[Pasted image 20261009134103.png]]

## Arbitrary Code Execution

![[Pasted image 20261009134537.png]]

We want to chain these to create a sort of turing machine, jumping between executable addresses each with "gadget". We end up with a weird but usable programming language. 

![[Pasted image 20261009134709.png]]
![[Pasted image 20261009134723.png]]
![[Pasted image 20261009134730.png|579]]

This writes `0x00000000` to the specified address.

# Summary
- A buffer overflow allows an attacker to write to the stack, for example letting them control the return address.
- If shellcode cannot be executed directly as the stack is non-executable, then instead we can jump into existing code:
	- Existing `libc` functions,
	- Chain of gadgets.
- Modern exploits often use some form of Return Oriented Programming.
