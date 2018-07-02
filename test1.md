# A Cryptographic Common Coin

The Common Coin produces a pseudorandom binary value that the correct nodes agree on, and that\
cannot be known beforehand.

Every Common Coin instance has a _nonce_ that determines the value, without giving it away: It
is not feasible to compute the output from the nonce alone, and for any "natural" sequence of
nonces (that can be feasibly computed), the output is uniformly distributed.
TODO: Bold claims! How much can we actually prove about that?

The nodes input a signal (no data, just `()`), and after _2 f + 1_ nodes have provided input,
everyone receives the output value. In particular, the adversary cannot know the output value
before at least one correct node has provided input.

## How it works

The algorithm uses a threshold signature scheme with the uniqueness property: For each public
key and message, there is exactly one valid signature. This signature can be produced using
signatures shares from any _2 f + 1_ holders of secret key shares.

* On input, a node signs the nonce and sends its signature share to everyone else.
* When a node has received _2 f + 1_ shares, it computes the main signature and outputs the XOR
of its bits.
