![[random_oracles_lecture_13.pdf]]

# MACs
Rememinder - prevents Eve from impersonating Alice

# MACs from Hash Functions
Often hash and macs we build don't line up in length.

# HMAC
Assuming we have a good hash function, we can build a MAC from it.

1. Could just hash the message and key and use the result as the tag
	1. Message and hash, and hash and message are both broken however.
## Prepend the Key
