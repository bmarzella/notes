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
	- *$P_{dynamic}$* = dynamic power, 