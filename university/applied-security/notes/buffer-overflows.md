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
### Initial Stack
Initially, the stack has three components, arranged as follows:
1. **Saved Return Address**
	- Holds the address of the next instruction in `main()` to be executed after `func()` (this function) finishes.
2. **Saved Frame Pointer** (`RBP`)
	- Stores memory address of the previous function's (`main()`, in this case) stack frame.
	- When `func()` finishes, it'll go back to `main90`
3. **Buffer \[0..15]**





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
