# Pegasus

![[Pasted image 20261009132553.png]]

- Exploit in WhatsApp allowed RCE which was leveraged for spyware.
- Marketed for law enforcement but was abused

# Buffer Overflow Recap

![[Pasted image 20261009132709.png]]

- C in fundamentally unsafe in the way it handles memory.
- For example, we can ride past the end off the buffer in this code to access data that shouldn't be.

## Stack on 64-bit x86

![[Pasted image 20261009132906.png]]

![[Pasted image 20261009132944.png]]
![[Pasted image 20261009133112.png]]
![[Pasted image 20261009133122.png]]
![[Pasted image 20261009133137.png]]
![[Pasted image 20261009133152.png]]
![[Pasted image 20261009133203.png]]
![[Pasted image 20261009133228.png]]
![[Pasted image 20261009133236.png]]

Whenever we call a function, the stack gets involved.

1. sad
2. asd
3. Function keeps track of the address it has to return to which is added to the stack, and the instruction pointer points to this.
4. Function saves base pointer and both RSP and RBP point to top of stack.
5. Create a buffer on the stack.
6. Insert "He"