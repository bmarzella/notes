**A pseudorandom function takes a key and input block and produces an output block.**
PRF takes seed to choose function? then feed inputs 1 -> x? chain outputs together?
# PRF Security Game
1. randomly generate a key
2. pass to PRF and truly uniformly random function
3. adversary is given output of either PRF or truly random function and tries to guess which one it was given
4. has access to an oracle of $f$, so can give queries without knowing which function.
5. can the adversary in polynomial time with polynomial queries gain an advantage.
$$
Adv^{PRF}_{F}  (𝒜) ≔ 2| Pr[𝑊] − \frac{1}{2} |
$$

If we query Q times, 
$$
\frac{q^2}{2} \cdot2^{-n}
$$
Block-cipher is a PRF if the number of queries allowed is less than root of output space
# PRF implies PRG
reminder, psuedorandom generator takes small truly random input, extends to psuedorandom output that is indistinguishable to an adversary from a truly random output.
# Stream Ciphers from Block Ciphers
