![[message_authentication_lecture_9.pdf]]
# Message Integrity & Authentication
Our eavesdropper Eve is capable of 
# Background: Beyond Confidentiality

**Confidentiality vs. Integrity & Authenticity**

**Real-world Active Threats**

# Core Authentication Goals

# Message Authentication Codes (MACs) Protocol

**Basic Protocol Steps**

**Key Properties**

# Formal MAC Definition

**Algorithmic Structure (S, V)**

**Correctness Requirement**

**Typical Verification Method**

# 2. MAC Security Models & Attacks

# Existential Unforgeability under Chosen Message Attack (EUF-CMA)

**Security Goal & Attack Model**

**Why Test Chosen Message Attacks?**

# MAC Forgeability Game

**Game Mechanics & Winning Condition**

**MAC Advantage Definition**

# Replay Attacks & Limitations

**Replay Attack Scenario**

**Why Basic MACs Fail Against Replays**

**Common Protocol-Level Defenses**

# Timing Attacks on Verification

**Vulnerable Verification Logic**

**Attack Principle & Complexity Reduction**

**Xbox 360 Case Study**

**Mitigation: Constant-Time Comparison**

# 3. Constructing MACs from PRFs

# Fixed-Length MAC from PRF

**Construction Details**

**Security Theorem & Advantage Bound**

**Primary Limitation**

# Flaws in Naive Variable-Length Extensions

**Naive Block-wise Tagging Method**

**Attack Vectors**

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