---
handwriting-page-id: 8abe0380-454e-4836-a8de-91c8086fffe5
---
# Setup
Define $R_{-1} = L_0$. Because $L_i = R_{i-1}$, the ciphertext $(L_r, R_r)$ is $(R_{r-1}, R_r)$. All bit indices are mod $t$.
## The four rules
**1. Round rule (per bit)**

$$R_j[i] = R_{j-2}[i] \oplus \bigl(R_{j-1}[i+1] \vee K[i]\bigr)$$

**2. OR rule.** For an input bit $x[i+1]$ to $f$:

$$f(x,K)[i] = x[i+1] \vee K[i] = \begin{cases} K[i] & \text{if } x[i+1] = 0 \\ 1 & \text{if } x[i+1] = 1 \text{ (nothing learned about } K[i]\text{)} \end{cases}$$

**3. XOR can be undone.** Rearranging rule 1:

$$R_{j-2}[i] = R_j[i] \oplus \bigl(R_{j-1}[i+1] \vee K[i]\bigr)$$

**4. Known values**

$$\text{Plaintext: } R_{-1},\, R_0 \qquad \text{Ciphertext: } R_{r-1},\, R_r$$

Everything $R_1, \dots, R_{r-2}$ is unknown.
## Check: $r = 3$
Forward from the plaintext (rule 1, $j=1$):

$$R_1[i] = L_0[i] \oplus \bigl(R_0[i+1] \vee K[i]\bigr)$$
Backward from the ciphertext (rule 3, $j=3$):

$$R_1[i] = R_3[i] \oplus \bigl(R_2[i+1] \vee K[i]\bigr)$$
Both equal $R_1[i]$, so

$$L_0[i] \oplus R_3[i] = \bigl(R_0[i+1] \vee K[i]\bigr) \oplus \bigl(R_2[i+1] \vee K[i]\bigr)$$
If $R_0[i+1] = R_2[i+1]$, the two terms are identical, so $K[i]$ cancels and you learn nothing. If exactly one of them is $1$, the right side equals $\lnot K[i]$.