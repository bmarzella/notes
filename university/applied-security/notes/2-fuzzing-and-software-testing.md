![[fuzzing-and-software-testing.pdf]]

# Fuzzing & Software Testing

## 1. Testing Foundations & Box Model
### Tests
**Unit Tests** - Testing isolated "units" of code, such as individual functions.
Does `add (1,1)` return `2`?
**Intergration** - Testing units combined together.
Does `Sub(Add(1,1),2)` return `0`
**System** - Testing the full system.
**Acceptance** - Is the user happy. 
### Boxes
**Black** - No information about internal structure. 
What an adversary would probably be facing.
**Grey** - Some information about internal structure, you might know `strcopy()` is used.
Or, internals might be too complicated.
**White** - Full access
### Static vs Dynamic
**Static** - don't execute, looking at code to find potential vulns.
**Dynamic** - executing and observing behaviour to find vulns. need to smartly select inputs to cover whole codebase.
## 2. Code Coverage Types
How much code am I covering with tests? `add(1,1)` is barely any coverage, for example.
Want to cover as much code as we can with our tests, ideally.

Nodes are instructions without a branch. Edges are the branches.

**Function** - Which functions are executed
**Line** - Which lines of source code 
**Block** - Machine code (the graph nodes)
**Edge** - Transitions (graph edges)

Graph highlights in side 16 example the missing `break;`, one of the `ifs` ends up in the `default`.
For function coverage, we'd need three tests, `AX - handle_A()`, `BX - handle_B_default()` `BS = handle_B_special()` and `housekeeping()`.

*? how do we know the number of each, pretty intuitive but just clarify*
### Taint Analysis
Put a flag on some attacker controlled input, track whether it ends up somewhere dangerous.
How fare does attacker controlled value reach into the program.
### Symbolic Execution
Symbolic execution means running a program using **symbols (variables)** instead of concrete inputs (like numbers or text).

As the program executes, every `if/else` statement or check adds a mathematical rule (a condition) to a list. By the end, the engine has a formula that answers:

> *What inputs do I need to feed this program so it satisfies all the conditions required to reach line X?"*
## 3. Fuzzing Engine & Loop
- Feeding random inputs and seeing what happens
### Feedback-driven
- In modern programs we use feedback from fuzzing runs to guide test creation
- For most feedback, we need some feedback
	- For example, if we are white box testing, we can use a special compiler that can guide the fuzzer
### Coverage-Guided
- Most common
- Fuzzer is biased towards new paths 
### Instrumentation
- Modifying the code to change its functionality, in our case we use this to help the fuzzer.
	- Re-writing the binary so it does the same thing but keeps some extra tracking information, for example
### Input Creation
#### Mutation-based
- Start somewhere, random string, valid input, set of valid inputs etc
- Apply mutations, flip a bit, increase length etc
- See what that does to coverage, if it's good for coverage, bias mutation in that direction
- Usually doing this
### Generation-based
- If we know input structure, ????????????

### Target
#### Binary
- Challenging to instrument, difficult to modify binary without changing function
- 
#### Source
- 
### Terminology
- Harness, wrapper for target the fuzzer targets
- Seeds, initial corpus
### Algorithm
Fill queue with test inputs, record the results of the inputs, once this set of inputs is done, pick at least one to base our next next set of inputs on
## AFL (++)

![[Pasted image 20261023133313.png]]
hello bro <3

- Coverage, mutation based
- Fast, reliable, easy to use
- More than just a fuzzer
### What does it do
- afl-fuzz provides test inputs to target
- executes and tracks tests
### Instrumentation
- Replaces compiler with afl's compiler
- Run afl on result (not the original binary!)
### Mutation Strategies
- Trim - Make test as short as possible without reducing coverage
- Increasing or decreasing numbers
- Flipping bits
- Combination of multiple strategies, deleting blocks, duplicating blocks etc
- Can run multiple fuzzing processes in parallel and swap test cases between
### Sanitisers
- Want to tell apart bugs and not bugs
	- Crash = bug, but a bug doesnt always cause a crash
- Sanitisers (instrumentation helps too) try to identify what is and isnt a bug
- Address sanitiser is probably the most relevant
	- Helps us find memory issues
## Roadblocks
- Sometimes a fuzzer will get stuck at a branch and miss a lot of functionality, we have ways to address this
## 4. Practical Tooling (AFL++ / ASan)
- 

## 5. Confusing Points / Slide Numbers
-