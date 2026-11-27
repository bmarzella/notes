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
- Pirating TV pre-streaming. Could, for example, clone sky viewing cards that would allow for TVs to locally decrypt paid TV signals.
- Chip tuning, removing software enforced speed limits on cars
## Fault Models
How can the attacker impact internal values?
### Spatial Location
Can we touch a specific bit/byte/word/instruction etc or a random position/width.
### Temporal Positioning
Can we effect just one cycle, one instruction, or is it more random, multiple cycles/instructions, is there jitter so we can't consistently hit the same stuff.
### Fault Effects
- Flipping bits
- Getting bits stuck
- Statistical -> 75% of the time we flip a bit
- Skipping instructions
## Control Flow Manipulation & Logic Bypassing
### Bypassing PIN Checks & Branch Instruction Flipping
Could have a `memcmp` to check a pin that runs `return -1;` i the pin is wrong.
If our fault model allows us to skip a single, specific instruction, we could skip the pin if check!
## Cryptographic Fault Attacks
### Generic Attacks on Keyed Algorithms
On any encryption function ciphertext = e(key, plaintext)
#### Attack Assumptions & Methodology
- We can repeatedly call the function for some constant plaintext, knowledge of which isnt necessary
- We can force any bit of k to be stuck as 0
#### Attack
1. Get fault free ciphertext
2. Inject fault bit 0 for the first key bit:
	1. If ciphertext is the same, then we know nothing changed, therefore the first bit is 0
	2. If ciphertext is different, key mustve changed, therefore first bit was 1
3. Repeat for remaining key bits
#### Practical Implications & Extensions
1. Simple, but effective and practical
2. Works the same for stuck-at-one
3. Can be extended for other fault models:
	1. If we can only force all-one or all-zero, then we can eliminate have of our search space for a brute force
	2. Can work byte-wise too
### Differential Fault Analysis (DFA) on Block Ciphers
### CRT-RSA Fault Attacks
#### Mathematical Foundations of CRT-RSA
$\text{Signature} = \text{Plaintext}^{key} \space \text{MOD} \space n$
Where $n$ is the product of two primes.
#### Fault Model
Random location, any effect, one *subexponentiation*, we just have to hit it. Fault propagates down. 
#### Bellcore Attack
If we get a normal and faulty signature, then we can take the difference and find the greatest common devisor with $n$, which yields one of our primes, from which we can find the other.
$$
q = \text{gcd}(s-s', \space n), \space p = \frac{n}{q}
$$
*Recording will go through full process*
#### Lenstra Attack
Improvement on bellcore, not always realistic to be able to get a faulty signature on the same value:
$$
q = \text{gcd}(s^{-e} - x, \space n), \space p=\frac{n}{q}
$$
### Accidental Faults & Real-World Vulnerabilities

## Software-Based Fault Injection
### Rowhammer Attacks
Access a single memory location over and over could cause bitflips, which could be leveraged.
## Countermeasures
### Detection-Based Defences
- Looking for things like EM, voltage changes etc
- L

### Algorithmic Countermeasures

### Pitfalls & Implementation Weaknesses in Countermeasures

## Summary & Take-Home Points