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
	- Old microcontrollers had the ability to reset fuses by shining UV light, but need to cover other components like flash to avoid wiping stuff we care about 
		- Manufacturer put a shield over it, so required tiltinng light source to reflect underneath
- ### Dump & Disassembly:
	- 
    
# Extraction Techniques: Complex Devices

## Software & Network Methods
### Manufacturer Sources:
### Network Traffic Analysis:
### Exposed Network Services:
### Hardware Interfaces

- **UART (Universal Asynchronous Receiver-Transmitter):**
    
    - _Pinout:_
        
    - _Exploitation / Bootloaders:_
        
- **JTAG / SWD / Test Interfaces:**
    
    - _Automated Pin Discovery Tools:_
        
- ## **Direct Flash Access / Chip-Off:**
    

## 4. Exploiting Bootloaders & Logic Bugs

- ## **Bootloader Vulnerabilities & Glitching:**
    
- ## **Case Study (e.g., LPC1343 CRP ROP Exploit):**
    

## 5. Relevant Hardware Weaknesses (CWE Top Weaknesses)

- **CWE-1191:**
    
- **CWE-1234:**
    
- **CWE-1244:**
    
- **CWE-1247:**
    

## 6. Key Takeaways & Summary Points