We have a non invertible hash function. Attacker can compute forwards, not backwards.

1. Attacker takes an arbitrary input and hashes
2. Hashes the result, hashes that etc
3. Eventually, ends up with a graph that leads back to the original input
	1. Because hash has one input, one output