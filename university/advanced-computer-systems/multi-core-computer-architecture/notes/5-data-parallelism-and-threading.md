![[Lec2.pdf]]****
# Lecture 2: From Vectors to Heterogeneous Architectures
## 1. Out-of-Order (OoO) & Flynn's Taxonomy
### OoO Stalls
1. Instruction actually needs result of earlier uncompleted instruction
2. Run out of spare registers to bypass false dependencies (WAW/WAR - register renaming)
3. Re-order buffer is full, core cannot dispatch new instructions
4. Changing core frequency changes the number of instructions completed per second, not per cycle
### Flynn's Taxonomy
- **Single Instruction, Single Data (SISD):** One instruction operates on one piece of data at a time.
  - Traditional single-core CPU
  - One thing at a time
- **Multiple Instruction, Single Data (MISD):** Multiple different instructions operate on a single data stream.
  - Rare, mainly used for redundancy like space flight computers
  - Core 1: `add x, y`; Core 2: `sub x, z`
- **Single Instruction, Multiple Data (SIMD):** One instruction applied to multiple data pieces at the same time across multiple processing units.
  - GPUs
  - Lane 1: `add x, y`; Lane 2: `add a, b`
- **Multiple Instruction, Multiple Data (MIMD):** Multiple independent processors execute different instructions on different data at the same time.
  - Multi-core CPUs
  - Core 1: `add x, y`; Core 2: `sub a, b`
---
## 2. SIMD & Vector Processors
### Foundations
- **Data Parallelism (SIMD):** Same instruction $\rightarrow$ Different Data
- **Data Flow (Unique):** Instruction runs whenever input data arrives
- **Thread Parallelism (MIMD):** Completely independent instructions and tasks running side by side
- SIMD is a fast way to apply one single command to a bunch of data points all at once—fry cook with one massive spatula to flip every burger at once.
- We feed SIMD hardware multiple data points as vectors.
- SIMD hardware requires multiple Processing Elements (PEs: physical execution units that perform work).
### Execution Strategies
- **Space (Array Processor):** Multiple physical side-by-side units at the same clock cycle.
  - 4 chefs with their own grill flipping their single patty at the same time.
- **Time (Vector Processor):** One physical unit, streams items through consecutively.
  - One chef with one grill, burgers constantly being placed on grill.
- Both carry out a single instruction over the entire vector. We can either spread work across space (more hardware but all at once) or time (more time but less hardware).
### Mechanics & Assembly Syntax
- Each vector register holds a vector of size $n$, values are fed into the PE one-by-one and the result for each cycle is fed into an output vector register.
- *Note on Slide 10 symbol:* Square symbol is meant to be `<-` (e.g., `LD VR <- A[3:0]`).
- `LD VR <- A[3:0]` $\rightarrow$ Load each element of array A into register `VR`.
- `ADD VR <- VR, 1` $\rightarrow$ Add one to every element in `VR`.
- We can see that the array processor is able to process the same operation at the same time across multiple PEs, whereas the vector uses pipelining: different PEs work on different instructions, but the result is the same instruction being applied to each element of the vector.
- Remember that we're talking about the processor as a whole. Even though vector is using multiple different PEs to complete multiple different instructions, which feels like MIMD, the end result is that the same instruction is being applied to each element of the array.
### Special Registers
- **Vector Registers:** Load/Store vectors.
- **Vector Length Register (VLEN):** How many elements of a vector are we processing (e.g., hardware max capacity might be 64, but we only care about processing 10).
- **Vector Stride Register (VSTR):** How far apart data elements are in memory (e.g., 1 for contiguous; used specifically when transferring data between memory and VRs; inside the VRs, elements are aligned in order).
### Matrix Multiplication
Memory is 1-dimensional but matrices are 2D. We store each row/column of a matrix as a vector.
- **Loading Matrix A:**
  - We want to load row 0 into $V_1$.
  - These values are stored contiguously in memory.
  - Therefore, we use a stride value of 1.
- **Loading Matrix B:**
  - We want to load column 0 into $V_2$, because matrix multiplication is $\text{row} \cdot \text{column}$.
  - Matrix B is 10 items wide, so column elements sit 10 spots apart in memory.
  - Therefore, we use a stride value of 10.
### Advantages
Vector instructions allow pipelines to run fast and deep because vector operations are predictable and free of traditional bottlenecks:
1. **No Data Dependencies:** Computing element #2 has nothing to do with the answer for element #1, so we never have to stall.
2. **No Branching:** Vectors never have if/else, so path is always linear from start to finish.
3. **Smart Prefetching:** Processor knows the stride in advance and it never changes, meaning it can calculate all memory locations it will need and fetch the data from memory into cache before it actually needs it.
*To be clear, this isn't a replacement for CPUs. Vector operations never have to branch, so vector hardware cuts out the ability altogether. For a GPU that only has to do vector multiplication, this is great, but it literally can't do what a CPU can.*
#### Additional Advantages:
- No dependencies or loops
- Fewer branches (no branches inside vectors themselves, but we still need to branch in the overall program code)
- Regular memory access pattern
- **High instruction efficiency and low control overhead:**
  - The processor only has to fetch, decode and issue one instruction instead of 50.
  - Cuts down on fetch bandwidth.
### Disadvantages
1. **Requires Regular Parallelism:** Only works when identical instructions are being applied to predictable memory locations. Fails on a linked list, for example.
2. **Memory Bandwidth Bottleneck:** Processes math so fast that RAM often cannot feed it data quickly enough.
   - **Compute/Memory Imbalance:** Too many load/store instructions relative to heavy math, loading an entire vector just to add 1 and store it back.
   - **Memory Bank Conflicts:** Memory is split into independent channels (banks) so multiple values can be read at once. If a stride pattern means that elements of the vector are stored within the same memory bank, memory is forced to handle them sequentially.
     - Example: Say $V_1$ is made up of elements X, Y, and Z, and memory banks are split every 3 positions (which wouldn't be realistic, but just for argument's sake), however X, Y, and Z are stored contiguously. This means X, Y, and Z are all part of the same memory bank, so we have to fetch one element per cycle instead of fetching all three at once.
---
## 3. Amdahl's Law & Multithreading
### Amdahl's Law & Serial Bottleneck
- Calculates the maximum performance improvement we can get by running parts of a program in parallel.
- No matter how many cores we throw at a task, we are capped by the fraction of code ($1-f$) that must be run sequentially.
- Example: If 10% of our code must be executed sequentially, then we can never exceed a speedup of $10\times$.
### Multithreading Foundations
- Instruction level parallelism (ILP), executing multiple scalar (single instruction, single data) instructions from the same program at the same time, isn't enough for some workloads, such as database querying.
- **Thread Level Parallelism (TLP):** Runs multiple independent threads simultaneously, where each thread has its own instructions and data.
- **Hardware Multithreading:** Allows multiple threads to share execution units of a single core.
  - This requires each thread has its own state (registers, program counter, page table).
  - Fast execution, but thread-switching overhead can cause a bottleneck.
- ILP is a single core running one program and finding spots to overlap instructions, whereas multithreading lets that same core hold multiple programs at once so it can jump to another one whenever the current one gets stuck waiting.
### Fine-Grained Multithreading
Processor switches threads every single clock cycle. This ensures no instructions from the same thread are in the pipeline at the same time.
Instructions from the same thread are spaced far apart, so there is no need for complex logic checking data dependencies or branches within a thread. A single thread's previous instruction will complete before the next one is fetched.
- **Pros:**
  1. Allows for simpler hardware logic, no longer need to check for dependencies.
  2. Keeps the processor constantly busy by interleaving multiple independent tasks, meaning we have a high overall throughput.
  3. No need for branch predictors, thread decisions resolved before next instruction fetched.
- **Cons:**
  1. Individual threads run slower because they have to wait for their turn between cycles.
  2. Requires dedicated hardware to store state for every thread.
  3. Performance will drop if there aren't enough active threads to keep every cycle filled.
  4. Threads share cache and memory, so have to fight and can slow each other down.
  5. Instructions within a thread don't conflict, instructions between might, still need logic to handle memory access conflicts between threads.
### Coarse-Grained Multithreading
Processor runs a single thread and only switches when a major stall occurs.
- **Pros:** Keeps single-thread performance high.
- **Cons:** 
  1. Small stalls are unaddressed, losing throughput.
  2. When we do switch, pipeline must be flushed and state saved and the new thread takes time to fill it back up before it can begin completing instructions.
---
## 4. MIMD & Memory Architectures
### Shared Memory Models
Recall: MIMD is multiple independent cores running different instructions on different data at the same time.
- **Centralised Shared-Memory (CSM):** Every core has private cache, but shares main memory over a shared bus or switch.
  - **Uniform Memory Access (UMA):** Every core takes the same amount of time to access any location in main memory.
  - *In CSM, all cores share one big pool of main memory over a central wire. Every core gets its own fast cache, but because they all share that same main memory, access time is identical for everyone, though it quickly clogs up if you add too many cores.*
- **Distributed Shared Memory (DSM):** Memory is physically split and attached directly to individual nodes (processor + cache + local memory + I/O), all of which are linked by a global interconnection network.
  - **Non-Uniform Memory Access (NUMA):** Accessing local memory chunk is fast, but fetching from someone else's chunk over the network takes longer.
  - *DSM solves this by breaking memory into chunks and attaching a piece directly to each processor along with its own cache and I/O. These individual bundles are all linked together over a network, making it super easy to scale up, but it means accessing your local memory chunk is lightning-fast while reaching over the network to grab someone else's memory takes longer.*
### Modern Systems
- **Single-Chip Multi-Core Design:** Packing multiple cores onto one chip package.
- **Hybrid Memory Architecture:** Inside a single CMP, it uses CSM-style shared memory (a shared L3 cache and a shared memory controller so all cores on that chip access RAM equally).
- **Monolithic Dies:** Builds the entire CPU as one big single piece of silicon.
  - Physically big, gets expensive because a single defect ruins the entire chip.
- **Chiplets (Modern Shift):** Breaks CPU into smaller, modular dies (Core Complex Dies (CCDs)) and stitches them together onto a single package using fast interconnect bridges.
  - Smaller dies have higher manufacturing success rates, lower costs, allows mixing different dies (for example using the same CPU chiplet across different production lines).
---
## 5. Homogeneous CMPs
### Foundations of Homogeneous CMPs
- Every core is identical. Performance increased by increasing core number on same amount of silicon.
- UCA vs. NUCA is the same as UMA vs. NUMA, just applied to the shared L3 cache as opposed to main memory.
- **Cache Coherence:** Cores have private caches, so can hold their own copies of the same variable. Cache coherence is a hardware protocol that syncs them. If Core 0 changes value `x`, then the protocol updates or cancels Core 1's local copy so no core reads stale data.
### Interconnect Topologies
- **Shared Bus:** Everything is connected to the same central bus.
- **Ring Interconnects:** Data travels around loop, stopping at each core along the way before reaching destination.
- **Crossbar Switches:** Everyone is connected directly to each other.
- **2D Mesh/Torus NoCs:** Each core position has a router which guides data to its goal.
### OS Scheduling in Homogeneous CMPs
- How does the OS decide which core runs which thread? Cores are identical, so any thread can run on any free core.
- If Core 0 is idle and Core 1 has a long queue, Core 0 will steal a task from Core 1 to keep all cores busy (work stealing).
- OS prefers to keep a thread on the same core, moving threads forces the core to reload its data onto the new core's private cache (cold miss).
- Managing one single job queue for dozens of cores causes slowdowns because cores fight to lock and read queue at the same time.
- **Amdahl's Law in Symmetric CMPs:** In homogeneous CMPs, the single core that has to handle the sequential code (our 10% from earlier) handles it alone, meaning the remaining cores sit idle, wasting energy and leaking static power.
---
## 6. Heterogeneous / Asymmetric Multiprocessing (AMP)
### Intro to Heterogeneous Computing
- Cores do different things on the same silicon.
- Cores speak same language so programs won't crash when moved between them.
- Big OoO cores and small in-order cores.
- Serial programs sent to OoO cores and background work to small cores.
### Hill & Marty
- **BCE (Base Core Equivalent):** The unit used to measure chip budget, where 1 standard small core = 1 BCE.
- **Pollack's Rule:** Making a core $2\times$ bigger only makes it about $1.4\times$ faster ($\sqrt{\text{area}}$).
- **1 Big Core + Many Small Cores:** Uses 1 big core to crush the slow serial code, and fills the remaining chip space with tiny cores for parallel work.
- **Asymmetric Superiority:** Mathematically proves this mixed approach beats using all identical cores.
### Arm big.LITTLE & DynamIQ
- **"big" Cores (Cortex-X / A7xx):** High-power OoO cores built for fast burst speed and heavy interactive tasks.
- **"LITTLE" Cores (Cortex-A5xx):** Low-power efficient cores built for background tasks and audio/video playback.
- **DynamIQ Shared Unit (DSU):** Puts both big and LITTLE cores into the exact same group (cluster) sharing one L3 cache, instead of separating them.
- **Zero-Copy Task Migration:** Moving a task between big and LITTLE cores takes nanoseconds because data stays right there in the shared L3 cache.
### Intel Hybrid x86 Architecture
- **P-Cores (Performance):** Big OoO cores with Hyper-Threading (SMT) and private L2 caches for heavy single-thread performance.
  - *SMT (Simultaneous Multithreading):* A hardware feature that allows a single physical core to act as two logical cores. For example, when you see a CPU advertised as 8 cores, 16 threads.
- **E-Cores (Efficient):** Small cores grouped in 4-core clusters (no Hyper-Threading, shared L2) designed to fit as many cores as possible in small area.
- **Unified Ring Bus:** Connects P-cores and E-core 4-clusters together on a single ring to share the main L3 cache (LLC).
- **ISA Parity Enforcement:** Disables instruction features that E-cores lack (like AVX-512) so both core types execute the exact same binary instructions.
### Intel Thread Director Telemetry & Energy Scheduling
- Hardware dedicated to analysing and classifying threads so the OS doesn't have to do the work of deciding where to send them.
- **Embedded Hardware Telemetry:** On-chip sensors monitor running code in real time to see what types of instructions it uses (math, memory stalls, etc.).
- **Dynamic Execution Classification:** Classifies each thread on the fly based on whether it needs heavy compute power or spends time waiting on memory.
- **EHFI (Hardware Feedback Interface):** Directly tells the OS scheduler which core type is best suited for each thread.
- **Intelligent Steering:** Hardware automatically routes heavy math/vector loops to P-cores and shifts idle background tasks to E-cores.
- **Active Energy Model (EM):** Keeps a lookup table of how much power each core type consumes at different speed/voltage levels.
- **Predictive Energy Evaluation:** Calculates the energy cost of running a task on a small core vs. waking up a sleeping big core.
- **Capacity-Aware Placement:** Assigns the thread to the most power-efficient core that can still finish the job on time.
- **Hysteresis Thresholding:** Prevents threads from rapidly bouncing back and forth between cores ("ping-ponging"), avoiding wasted time and cache misses.
### Heterogeneous Cache Coherence
- Big cores process so much data that their cache traffic can flood and overwhelm the smaller cache ports on little cores.
- A central directory filters out unnecessary cache checks so little cores don't waste time handling requests for data they don't hold.
- Both big and small cores must follow the exact same memory ordering rules (like TSO on x86 or Weak Ordering on ARM) so memory ops don't execute out of sync.
- Internal hardware queues handle memory writes and locks automatically to prevent race conditions across different core types.
  - *Race conditions:* When two or more threads/cores try to access and change the same piece of data at the same time, and the final outcome depends on who got there first.
### The Heterogeneous Compute Future
- Mixing big and small CPU cores is now the standard design everywhere from phones to cloud servers.
- Chips combine CPUs with specialized units (NPUs, GPUs, DSPs) that process specific tasks using far less power to avoid dark silicon limits.
- 2.5D and 3D packaging technologies allow connecting separate silicon chiplets built on different manufacturing nodes into one chip package.
- Hardware telemetry engines and OS schedulers must be designed together so the OS knows exactly when and where to route work across all specialised units.