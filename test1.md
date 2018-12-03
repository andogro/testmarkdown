# Verifiable Delay Function (VDF)

## What is a VDF?

A Verifiable Delay Function (VDF) is a function that requires substantial time to evaluate (even with a polynomial number of parallel processors) but can be very quickly verified as correct. VDFs can be used to construct randomness beacons with multiple applications in a distributed network environment. By introducing a time delay during evaluation, VDFs prevent malicious actors from influencing output. Output cannot be differentiated from a random number until the final result is computed. 

See https://eprint.iacr.org/2018/712.pdf for more details.

## Description

This VDF implementation is written in Rust. We use class groups to implement 2 approaches.
1. [Simple Verifiable Delay Functions](https://eprint.iacr.org/2018/627.pdf). Pietrzak, 2018
2. [Efficient Verifiable Delay Functions](https://eprint.iacr.org/2018/623.pdf). Wesolowski, 2018

This repo includes three crates:

* `classgroup`: a class group implementation, as well as a trait for class groups.
* `vdf`: a Verifyable Delay Function (VDF) trait, as well as an implementation of that trait.
* `vdf-cli`: a command-line interface to the vdf crate. It also includes additional commands, which are deprecated and will later be replaced by a CLI to the classgroup crate.

## Usage

- Install [Rust](https://doc.rust-lang.org/cargo/getting-started/installation.html)
<!--- Any Version?
--->

- Install the [GNU Multiple Precision Library](https://gmplib.org/)
   ```
   sudo apt-get install -y libgmp-dev
   ```
- Download and prepare the repository
   ```
   $ git clone https://github.com/poanetwork/vdf.git
   $ cd vdf
   $ cargo install
   ```

### Command Line Interface

To initiate, use the `vdf-cli` command followed by 2 arguments.

- _challenge_: byte string of arbitrary length
- _difficulty_: number of iterations, each iteration requires more time to evaluate

<!---
-Is this the wesolowski proof of time, are there ways to run the other proofs?
--->

Once complete you will see the output,returned as a `Vec<u8>`.

**Example**
```
$ vdf-cli aa 100
005271e8f9ab2eb8a2906e851dfcb5542e4173f016b85e29d481a108dc82ed3b3f97937b7aa824801138d1771dea8dae2f6397e76a80613afda30f2c30a34b040baaafe76d5707d68689193e5d211833b372a6a4591abb88e2e7f2f5a5ec818b5707b86b8b2c495ca1581c179168509e3593f9a16879620a4dc4e907df452e8dd0ffc4f199825f54ec70472cc061f22eb54c48d6aa5af3ea375a392ac77294e2d955dde1d102ae2ace494293492d31cff21944a8bcb4608993065c9a00292e8d3f4604e7465b4eeefb494f5bea102db343bb61c5a15c7bdf288206885c130fa1f2d86bf5e4634fdc4216bc16ef7dac970b0ee46d69416f9a9acee651d158ac64915b
```
To verify, use the `vdi-cli` command with the same arguments and include the output.

**Example**
```
$ vdf-cli aa 100 005271e8f9ab2eb8a2906e851dfcb5542e4173f016b85e29d481a108dc82ed3b3f97937b7aa824801138d1771dea8dae2f6397e76a80613afda30f2c30a34b040baaafe76d5707d68689193e5d211833b372a6a4591abb88e2e7f2f5a5ec818b5707b86b8b2c495ca1581c179168509e3593f9a16879620a4dc4e907df452e8dd0ffc4f199825f54ec70472cc061f22eb54c48d6aa5af3ea375a392ac77294e2d955dde1d102ae2ace494293492d31cff21944a8bcb4608993065c9a00292e8d3f4604e7465b4eeefb494f5bea102db343bb61c5a15c7bdf288206885c130fa1f2d86bf5e4634fdc4216bc16ef7dac970b0ee46d69416f9a9acee651d158ac64915b
Proof is valid
```

### VDF Library
<!---
Keep as is, and possibly include argument explanations as well (for byte_length for example). May not be needed though is CLI is main user interaction tool.
--->

## Benchmarks

Benchmarks are provided for the classgroup operations. To run benchmarks:

```
$ cd vdf/classgroup
$ cargo bench
```

Additional benchmarks are under development.

### Current Benchmarks
<!---
Include table, similar to https://github.com/Chia-Network/vdf-competition#benchmarks
--->
## License

Apache License, Version 2.0, (LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0)

<!---
Note - According to contest rules all source code must be made in pursuant to the terms of the Apache license. 
You can copy and include LICENSE-APACHE from threshold-crypto crate. Not sure about MIT license, lets confirm.
--->
