# PRF implies PRG
reminder, psuedorandom generator takes small truly random input, extends to psuedorandom output that is indistinguishable to an adversary from a truly random output.

**A pseudorandom function takes a key and input block and produces an output block.**

# PRF Security Game
1. randomly generate a key
2. pass to PRF and truly uniformly random function
3. adversary is given output of either PRF or truly random function and tries to guess which one it was given
$$
Adv^{PRF}_{F}  (𝒜) ≔ 2| Pr[𝑊] − \frac{1}{2} |
$$

