![[message_authentication_lecture_9.pdf]]
# Message Integrity & Authentication
Our eavesdropper Eve is capable of not just listening in, but also modifying. **We want to protect the message from modification.** When Bob receives a message, he wants to know it was Alice that sent it.

Suppose Alice sends a message to the bank, `transfer $100 to bob` (suppose this is plaintext to make things easier). Eve could:
1. intercept and modify this directly: `send 0 to bob`
2. add their own block, so if message to bank is split between `send $100` and `to bob`, Eve could send `0000` between the two.

Alternatively, let's look at a MITM, Charles. If Alice and Bob share a private key. When Alice sends her message to Bob, she sends the message and a **tag** t, which is computed using the key. Bob has a **verifier v**, which checks whether the tag is valid. (If the message were encrypted, we'd decrypt first, then verify the tag, but we're just focusing on Integrity and Authenticity).

**The tag should not be forgeable.** Only someone with the key can generate/verify a tag.
# Background: Beyond Confidentiality
**Confidentiality vs. Integrity & Authenticity**
So, we have three goals:
- **Confidentiality:** Keeping message secret, cryptography as we've seen so far.
- **Intergrity:** Message hasn't been modified
- **Authenticity:** Message comes from who we think it does
- **Non-repudiation:** Sender cannot deny sending
	- Alice cannot claim that she didn't send anything to the bank, because we know she did.
# Core Authentication Goals
## Message Authentication Codes (MACs) Protocol

**Basic Protocol Steps**

**Key Properties**

# Formal MAC Definition
![[Pasted image 20261106161951.png]]

**Algorithmic Structure (S, V)**
- Sign is a function that takes the key and message, and produces some tag.
- Verify is a function that takes the key, message and tag and accepts if unmodified and rejects if not. 
	- V can just = S, if Bob produces the same tag, then there cannot have been modification.
**Correctness Requirement**
???
**Typical Verification Method**
???
# MAC Security Models & Attacks
## Existential Unforgeability under Chosen Message Attack (EUF-CMA)
### Security Goal
Adversary cannot forge valid tags.
### Attack Model
1. Adversary is able to request tags for messages of their choosing through an oracle.
2. Adversary wins if it produces a valid tag for a message they have not passed to the oracle.
3. We assume that the adversary, being an eavesdropper, would be able to collect lots of message-tag pairs
### Why Test Chosen Message Attacks?
- ???
## MAC Forgeability Game
### Game Mechanics
???
### Winning Condition
- Attacker can fool verifier
- Message was never given to oracle
**MAC Advantage Definition**
$$
Adv_{J}^{MAC}(\mathcal{A}) := Pr[\mathcal{A}\space\text{wins}]
$$
# Replay Attacks
**Replay Attack Scenario**
- Eve grabs an authenticated message with a valid tag and repeatedly sends it.
- For example, if Alice sends `send $1000 to bob`, Eve can just repeat this, and the bank will see it as legitimate.
**Why Basic MACs Fail Against Replays**
- Verification exists in isolation, we don't look at previous messages
- Same message = same tag
**Common Protocol-Level Defenses**
- Time-stamp
- Number messages
- Add a nonce (random value per)
- Tie ? to a short lived session
# Timing Attacks on Verification
1. If tag verification is done byte-by-byte
	- Let's say each byte takes a second to verify
2. Because verification is byte-by-byte, attackers could just check every byte. If they got it correct, it'd move onto the next byte. If not, it would immediately reject within a second.
3. Thus, pirates could use the time it takes to verify each byte to brute force the tag!
**Xbox 360 Case Study**
4. Pirates would clone Xbox game disks and sell them for cheaper than legit copies.
5. So, tags were added to game disks.
6. 
**Mitigation: Constant-Time Comparison**
# Constructing MACs from PRFs
## Fixed-Length MAC from PRF
**Construction Details**

**Security Theorem & Advantage Bound**

**Primary Limitation**
- Only handle $n$ bit messages
- In reality, messages will vary in length.
## Naive Variable-Length Extensions
### Naive Block-wise Tagging Method
Split messages into blocks, tag each separately.
1. Turn message into blocks
2. Compute tag for each
Each block independently is seThis is very weak!
### Attack Vectors
Recall that 
1. 
# Secure Variable-Length Construction

**Metadata Countermeasures**

**Sign and Verify Algorithms**

**Security Proof Cases**

**Efficiency Drawbacks**

# 4. CBC-MAC

# Basic CBC-MAC Construction

**Algorithm Steps**

**Efficiency Metrics**

# CBC-MAC vs. CBC Encryption

**Key Structural Differences**

# Critical Design Rules

**Why Fixed IV (**$0^n$**) is Mandatory**

**Attack on Random-IV CBC-MAC**

**Why Only Final Block Output is Allowed**

**Attack on Intermediate-Output MACs**

# Handling Variable-Length Messages in CBC-MAC

**Vulnerability of Basic CBC-MAC to Variable Lengths**

**Method 1: Length Prefixing**

**Method 2: Two-Key Approach**

**Standards (NIST FIPS 113, ISO/IEC 9797-1)**

# 5. Core Takeaways & Summary

**Essential MAC Security Requirements**

**Key CBC-MAC Guidelines**