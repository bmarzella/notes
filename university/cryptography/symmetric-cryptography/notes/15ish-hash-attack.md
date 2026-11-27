We have a non invertible hash function. Attacker can compute forwards, not backwards.

1. Attacker takes an arbitrary input and hashes
2. Hashes the result, hashes that etc
3. Eventually, ends up with a graph that leads back to the original input
	1. Because hash has one input, one output
4. $2^n$ and $\sqrt{2^n}$ are important, not sure why
5. We store certain values (not sure which)

6. Finally, attacker is given a hash and wants to 