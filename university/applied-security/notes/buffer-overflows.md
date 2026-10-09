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

Whenever we call a function, the stack gets invovled.