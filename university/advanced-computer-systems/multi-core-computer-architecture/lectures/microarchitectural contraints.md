---
handwriting-page-id: e322e72d-b1e7-4e65-85d2-e2a824afefbc
---
![[Pasted image 20261006122604.png]]

# The Performance Triad
## Iron Law of Processor Performance

![[Pasted image 20261007103033.png]]

- Used to measure how fast a processor executes a program:
$$
CPU \space Time =\frac{Instructions}{Program} \times \frac{Cycles}{Instruction} \times \frac{Time}{Cycle}
$$
1. **Instructions per Program (Instruction Count)**: Total number of machine instructions requires to complete the program.
2. **Cycles per Instruction (CPI)**: Average number of clock cycles needed to execute each instruction.
3. **Time per Cycle (Clock Period):** Duration of a single clock tick, inverse of clock frequency.
## Latency vs Throughput

- **Latency** is the time required to complete a single task/instruction from start to finish (*critical for single thread responsiveness*)
- **Throughput** is the total amount of work completed per unit time across all execution units (*aggregate IPC, Flops/sec etc.*)
- Deep pipelining improves throughput by overlapping operations, but often increases individual instruction latency due to hazard penalties and pipeline register overheads. 
	- *Doing multiple things at once mean we get more done at once, but also means individual tasks can take longer.*

## The Third Dimension: Power & Energy Limits

- Faster clock frequencies and wider execution engines drive up power consumption:
$$
P_{total} = P_{dynamic} + P_{static} = \alpha \cdot C \cdot V^2 \cdot f + l_{leak} \cdot V 
$$
	- *$P_{total}$ = total power, combined power dissipated my the circuit. *
	- *$P_{dynamic}$ = dynamic power: Power consumed only when a logic gate is actively switching state ($1 \iff 0$)*
	- $P_{static}$ *= static power: Power continuously consumed when the circuit is powered on, even when idle or held at a cosntant state.*
	- $\alpha$ *= Activity/Switching Factor: The probability that a gate transitions during a given clock cycle.*
	- *$C$ = Load Capacitance: Total capacitance that must be charged and discharged.*
	- *$V$  = Supply voltage: Positive voltage applied to the circuit. Small reductions produce quadratic decreases in dynamic power.*
	- *$f$ = Clock Frequency*
	- $I_{leak}$ *= Leakage Current: Total wasted current flowing unwanted through other components.*

- Performance gains cannot be evaluated in isolation from **Thermal Design Power (TDP)** and **energy cost per instruction.**
- Optimisation targets have switched from raw execution speed to energy- delay metrics
	- We want the least delay using the least amount of energy, rather than pure speed. 

# Dennard Scaling

![[Pasted image 20261007104906.png]]

**Classical Promise**
-  If we shrink a transistors dimensions by a factor $s$ and lowering its operating voltage $V_{DD}$ by $1/s$, the transistor uses proportionally less power.
- Even though we fit $s^{2}$ more transistors into the same chip, the overall power density remained constant

**Free Lunch**
- Because power density stayed flat, we could:
	1. Fit $s^{2}$ more transistors on the same chip.
	2. Increase clock frequency by $s$, making processors faster.
- Processors got faster and denser without running hotter

**Voltage Wall**
- Could not reduce supply voltage forever. To turn a transistor on or off reliably, $V_{DD}$ must stay above threshold voltage ($V_{th}$).
- If $V_{DD}$ drops too low:
	1. Chip becomes sensitive to electrical noise
	2. $I_{leak}$ shoots up, causing massive power drain even when idle.
- Voltage scaling halted around 1 Volt.

**Power Wall**
- Transistor counts kept growing, but power leakage and constant $V_{DD}$ meant power requirements began growing.

**Architectural Shift**
- This culminated in the end of frequency scaling (around 304G)