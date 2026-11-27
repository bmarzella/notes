![[week19-fault-injection(1).pdf]]
## Introduction to Implementation Attacks
### Passive vs. Active Attacks
### Side-Channel Analysis vs. Fault Injection
#### Side Channel
Observations: temperature, power consumptions, timings etc
#### Fault Injection
Actions: Heating/cooling, shining lasers, messing with an external clock etc.
## Physical Fault Injection Techniques

### Power
For example, dropping supply voltage when target computation occurs could let use manipulate the result.
### Clock Glitching
For example, overclocking causes instability, so we can overclock for a single cycle to manipulate instructions.
### Electromagnetic (EM) Pulses
Generating EM pulses and shooting them at a chip.
### Laser and Optical Attacks
idk
### Hardware Setup & Real-World Examples
An example was for pirating TV pre-streaming. Could, for example, clone sky viewing cards that would allow for TVs to locally decrypt paid TV signals.

## Fault Models

### Spatial Location

### Temporal Positioning

### Fault Effects

## Control Flow Manipulation & Logic Bypassing

### Bypassing PIN Checks & Branch Instruction Flipping

### Software-Based Fault Targets

## Cryptographic Fault Attacks

### Generic Attacks on Keyed Algorithms

#### Attack Assumptions & Methodology

#### Practical Implications & Extensions

### Differential Fault Analysis (DFA) on Block Ciphers

### CRT-RSA Fault Attacks

#### Mathematical Foundations of CRT-RSA

#### Bellcore Attack

#### Lenstra Attack

### Accidental Faults & Real-World Vulnerabilities

## Advanced Targets & Software-Based Fault Injection

### Rowhammer Attacks

### Voltage & Frequency Scaling Manipulation

### Fault Injection in Machine Learning Models

## Countermeasures & Mitigation Strategies

### Detection-Based Defenses

### Algorithmic Countermeasures

### Pitfalls & Implementation Weaknesses in Countermeasures

## Summary & Take-Home Points