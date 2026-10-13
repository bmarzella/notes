---
handwriting-page-id: 302ce8a0-a835-414d-91a2-cdd77271522b
---
![[WS1 (1).pdf|1]]

# Slide 3
- outcome of each branch decision stored in branch history table 
- branch target buffer btb
- take decision from bht
- machine learning is heavy especially with a heavy history, god to have an accelerator

# Slide 4

How we implement branch prediction








- longer history = higher storage cost

**1 bit predictor**
- uses previous bit
- for example, for (1,10000), most of the time loop is taken

what about \[0101010101]? 1 bit wont work
**2 bit predictor**
- need an algorithm finite state machine
- we have 4 states, 00, 01, 10, 11
00 - strong not taken, 10 - weak not taken: either of these we predict the next branch will not be taken
01 - weak taken, 11 - strong taken: either of these we predict the branch will be taken

# Slide 5
three different cache memory mapping types
three sections of an address see slide
32 bits
\[tag | index | byte offset]
update btb on execution, update bht

# Slide 6
**Biased Loop**: *T T T T T T NT*
- 1-bit predictor mis predicts twice
- 2-bit predictor only mispredicts once per loop
**Alternating pattern**: *T NT T NT T NT T NT*
- 2-bit will always fail
- fixed with correlating predictor which requires gshare, global shared history table

# Slide 7
bht stores a single bit per decision, btb stores a lot more, therefore requires significantly more silicon area

# slide 9



