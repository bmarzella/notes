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
1. We begin by pointing `RBP` back to the address in Old RBP.
2. This discards the buffer, leaving `Return addr`.
3. Finally, we pop this off the stack and copy it to the `RIP` to have it point to the next instruction in `main`.




4. asd
5. Function keeps track of the address it has to return to which is added to the stack, and the instruction pointer points to this.
6. Function saves base pointer and both RSP and RBP point to top of stack.
7. Create a buffer on the stack.
8. Insert "Hello, World!". Does not exceed the length of the buffer so no issues.
9. Clear the stack and move on.

### Exploitation

![[Pasted image 20261009133406.png]]
![[Pasted image 20261009133427.png|583]]
![[Pasted image 20261009133547.png]]


1. Pass some  

### Mitigation
The first mitigation was to **Mark the stack as non-executable**. We can jump to wherever we want, but we cant execute code there.

![[Pasted image 20261009133746.png]]

#### What if we jump into existing code?
Instead of executing our own code, we could jump to existing code, say, *access granted.*

![[Pasted image 20261009133852.png]]
![[Pasted image 20261009133910.png|608]]

Put the stack into a state where the function works

### How are arguments passed to functions?

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
