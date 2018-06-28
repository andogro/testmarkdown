 # Broadcast

 The Reliable Broadcast Protocol assumes a network of `N` nodes that send signed messages to
 each other, with at most `f` of them faulty, where `3 * f < N`. Handling the networking and
 signing is the responsibility of this crate's user: only when a message has been verified to be
 "from node i", it can be handed to the `Broadcast` instance. One of the nodes is the "proposer"
 who sends a value. Under the above conditions, the protocol guarantees that either all or none
 of the correct nodes output a value, and that if the proposer is correct, all correct nodes
 output the proposed value.

 ## How it works

 * The proposer uses a Reed-Solomon code to split the value into `N` chunks, `f + 1` of which
 suffice to reconstruct the value. These chunks are put into a Merkle tree, so that with the
 tree's root hash `h`, branch `bi` and chunk `si`, the `i`-th chunk `si` can be verified by
 anyone as belonging to the Merkle tree with root hash `h`. These values are "proof" number `i`:
 `pi = (h, bi, si)`.
 * The proposer sends `Value(pi)` to node `i`. It translates to: "I am the proposer, and `pi`
 contains the `i`-th share of my value."
 * Every (correct) node that receives `Value(pi)` from the proposer sends it on to everyone else
 as `Echo(pi)`. An `Echo` translates to: "I have received `pi` directly from the proposer." If
 the proposer sends another `Value` message, that is ignored.
 * So every node that has received at least `f + 1` `Echo` messages with the same root
 hash will be able to decode a value.
 * Every node that has received `N - f` `Echo`s with the same root hash from different nodes
 knows that at least `f + 1` _correct_ nodes have sent an `Echo` with that hash to everyone, and
 therefore everyone will eventually receive at least `f + 1` of them. So upon receiving `N - f`
 `Echo`s, they send a `Ready(h)` to everyone to indicate that. `Ready` translates to: "I know
 that everyone will eventually be able to decode the value." Moreover, since every correct node
 only ever sends one kind of `Echo` message, this cannot happen for two different root hashes.
 * Even without enough `Echo` messages, if a node receives `f + 1` `Ready` messages, it knows
 that at least one _correct_ node has sent `Ready`. It therefore also knows that everyone will
 be able to decode eventually, and multicasts `Ready` itself.
 * If a node has received `2 * f + 1` `Ready`s (with matching root hash) from different nodes,
 it knows that at least `f + 1` _correct_ nodes have sent it. Therefore, every correct node will
 eventually receive `f + 1`, and multicast it itself. Therefore, every correct node will
 eventually receive `2 * f + 1` `Ready`s, too. _And_ we know at this point that every correct
 node will eventually be able to decode (i.e. receive at least `f + 1` `Echo` messages).
 * So a node with `2 * f + 1` `Ready`s and `f + 1` `Echos` will decode and _out
