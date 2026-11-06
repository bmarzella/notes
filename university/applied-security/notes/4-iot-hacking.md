![[week16-firmware-hacking.pdf]]
#firmware-extraction #firmware
# Overview & Fundamentals
## What is Firmware?
- Provides low level control for hardware
- Could have an OS, or just "bare metal" binary
	- Don't need linux to boot every time you unlock your car
## Why Extract Firmware?
- Looking for vulns and/or secret data
- reverse engineering
- forensics/failure anaylsis
	- why did my car drive into a wall
- repurposing eof devices
- developing replacement firmware
	- add features we want
# Extraction Techniques: Small / Invasive Devices
## Microcontrollers & Physical Attacks
- ### Decapping ICs: 
	- Want to extract embedded code from microcontrollers
	- Physically open the chip, for example with acid
- ### Bypassing Read-Out Protection:
	- Read-out protection - bit in memory that prevents reading out information
	- Fuses hold read-out bit
	- Old microcontrollers had the ability to flip bits by shining UV light, so we can flip read-out protection bit 
		- But need to cover other components like flash to avoid wiping stuff we care about 
		- Manufacturer put a shield over it, so required tiltinng light source to reflect underneath
- ### Dump & Disassembly:
	- Then we can dump contents of microcontroller!
# Extraction Techniques: Complex Devices
## Software & Network Methods
- More modern devices are more complicated and thus harder to secure
### Manufacturer Sources:
1. Manufacturer website
	1. Sometimes they'll just host it online and you can download
	2. Or email asking, legally they might have to
### Network Traffic Analysis:
1. If connected to network and they download updates from network, we can intercept this
	1. Might not be secured because why would it be
		1. There's a signature so it can't be MITMed, but we can read it
### Exposed Network Services:
1. Might have exposed network protocol
2. Or maybe we can just switch it on
	1. From there we can use exploits to gain root (unless it just gives us root)
### Hardware Interfaces
If none of this works, then we can start looking at hardware methods:

**UART (Universal Asynchronous Receiver-Transmitter):**
- Ancient interface still used today
Pinout:
- Two pins, receive and trasmit
	- Buy usb to pin converter, sometimes we can just press enter, or type root root, to gain root, for example
Exploitation / Bootloaders:
- U-boot
	- Can dump memory before linux loads
	- Maybe we can find root password hash, which we can then crack, for example
- Load custom bootloader
	- Might print some string while loading bootloader (such as `CCCC...`), which we can interrupt with our own binary
- **JTAG / SWD / Test Interfaces:**
    
    - _Automated Pin Discovery Tools:_
# Firmware extraction exploiting embedded bootloaders
- Sometimes microcontrolers are left unprotected because it makes debugging annoying
## Direct Flash Access / Chip-Off:
## 4. Exploiting Bootloaders & Logic Bugs

- ## **Bootloader Vulnerabilities & Glitching:**
    
- ## **Case Study (e.g., LPC1343 CRP ROP Exploit):**
    

## 5. Relevant Hardware Weaknesses (CWE Top Weaknesses)

- **CWE-1191:**
    
- **CWE-1234:**
    
- **CWE-1244:**
    
- **CWE-1247:**
    

## 6. Key Takeaways & Summary Points