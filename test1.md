# Project FAQs:

## Contents
- [What is Mana-Ethereum?](#what-is-mana-ethereum)
- [What are the project goals?](#what-are-the-project-goals)
- [What differentiates Mana from other Ethereum clients?](#what-differentiates-mana-from-other-ethereum-clients)
- [Why use Elixir?](#why-use-elixir)
- [What is the current project status?](#what-is-the-current-project-status)
- [Which chains will Mana-Ethereum support?](#which-chains-will-mana-ethereum-support)
- [What prerequisites are required to run Mana-Ethereum?](#what-prerequisites-are-required-to-run-mana-ethereum)
- [How do I run Mana-Ethereum?](#how-do-i-run-mana-ethereum)
- [How do I test?](#how-do-i-test)
- [Can I use Mana-Ethereum to mine Ether?](#can-i-use-mana-ethereum-to-mine-ether)
- [Where can I learn more about Ethereum, the yellow paper etc.?](#where-can-i-learn-more-about-etherem-the-yellow-paper-etc)
- [How can I contribute to the project?](#how-can-i-contribute-to-the-project)

### What is Mana-Ethereum?

Mana-Ethereum is an open-source Ethereum client written in [Elixir](https://elixir-lang.org/). An Ethereum client implements the Ethereum Virtual Machine (EVM), allowing the network node running the client to interact with the Ethereum blockchain and/or associated testnets and sidechains. 

The client must follow the specifications described in the [Ethereum Yellow Paper](https://github.com/ethereum/yellowpaper) to properly sync with the blockchain and verify block rewards, interact with smart contracts, and read and write transactions. 

The project is a [collaborative effort](https://medium.com/poa-network/poa-network-compound-and-consensys-announce-collaboration-on-ethereum-client-written-in-elixir-b265d048402) between [Compound](https://compound.finance/), [ConsenSys](https://consensys.net/), and [POA Network](https://poa.network/) designed to create a reliable, efficient, and easy-to-use EVM client.

### What are the project goals?

In the short term, our goal is to create a fully functional client comparable to Geth or Parity that can run day-to-day tasks on a network node. Once operational, we have several longer-term goals for this project.

**Contribute to client diversity:** It is important to have a diverse ecosystem of clients responsible for verifying EVM based blockchains. If there are issues with one client, nodes can quickly adopt another client to make sure the blockchain continues to function properly. This also creates a system of checks and balances to ensure transactions are correct and verifiable, prevents a monopoly by a single client, and provides the opportunity for multiple clients to innovate and optimize for different applications.

**Support alternative consensus methods, especially the PoA consensus model:** Proof of Authority consensus (which includes a small set of validators running nodes that utilize a BFT consensus algorithm) is well suited for blockchain networks with known, trusted validators. PoA is currently used in a public setting by POA Network, and many private chains also use PoA on their EVM based sidechains. Mana will support this consensus model for use in Proof of Authority based chains.

**Create a fast, low-memory node:**  With appropriate optimizations, we will leverage Elixir to create a fast and extremely reliable client. We will also optimize memory and storage handling to allow Mana-Ethereum to run on memory constrained devices. This will create new opportunities for users and expand network reach.

**Parallelize transactions:** Rather than relying on strictly sequential transactions, we will explore using [optimistic concurrency control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) to process transactions in parallel, further optimizing the speed and efficiency of the client.

### What differentiates Mana from other Ethereum clients? What makes Mana-Ethereum unique?

Mana-Ethereum’s vision is to create a fast, low-memory node that is easy to setup and functions optimally in a variety of environments.  We are creating a highly modular, well-documented client to promote transparency in the codebase and encourage collaboration. A point of emphasis with Mana-Ethereum is on reliability and near constant uptime. We believe these improvements will differentiate us from the current group of EVM clients.

Elixir provides us with tools and means to realize this vision. See Why use Elixir below for more information on our choice to leverage this optimized programming language.

### Why use Elixir?

We are choosing to implement Mana-Ethereum using Elixir for several reasons.

**Speed:** Elixir code runs inside lightweight, isolated processes, which allows thousands of processes to run at the same time. This concurrency creates a great deal of efficiency for all operations, including testing. Currently, we can run all state tests in less than 10 minutes.

**Scalability:**  Processes in one machine in the network are able to communicate with processes running on other machines in the network. This allows for an efficient, distributed environment that can quickly scale as needed. Elixir is known as a solution for high-traffic systems. Applications like Discord have used Elixir to handle [5 million concurrent users and process millions of events per second](https://blog.discordapp.com/scaling-elixir-f9b8e1e7c29b). 

**Resilience:** Fault-tolerance is built into Elixir. Supervisor and worker processes monitor the network for crashes to prevent system-wide failures. “Hot-swapping” allows code to be changed out without stopping the system.

**Time Tested:** Although Elixir is fairly new, it runs on the Erlang Virtual Machine, a platform that was first developed for distributed computing and telephone networks more than 30 years ago. 

**Efficiency:**  Elixir supports concise, easy-to-maintain code. Applications are highly modular, which is useful for a protocol with many different moving parts and wide-ranging functionality. Elixir is also designed to be more readable than Erlang. This creates fewer bugs and a faster ramp-up time for new developers.

### What is the current project status?

With the recently announced collaboration, the team has expanded and progress is happening quickly. We are currently passing 100% of the [Ethereum Common Tests](https://github.com/ethereum/tests) and rapidly closing in on a full Ropsten and mainnet syncs. Updated progress is available in the project [README](https://github.com/mana-ethereum/mana).

Currently, a main focus is on the ex-wire networking layer and peer to peer connection protocols (devp2p) which will enable a Mana-Ethereum node to discover and connect to other nodes, request blocks, and sync the blockchain with other nodes in the network.

### Which chains will Mana-Ethereum support? 

We are focusing our initial efforts to sync with the Ropsten testnet and the Ethereum mainnet. After these are synced, phase 2 will involve syncs with the following chains:
- Ethereum testnets
- Ethereum Classic
- Ethereum Classic-testnet
- POA Network
- PoA based private chains and sidechains

### What prerequisites are required to run Mana-Ethereum?

Software  
Elixir ~> 1.7.4

Hardware 
- We recommend atleast 4 GB RAM. 
- Disk space needs are based on the network size. To sync with the Ethereum Mainnet, we recommend 2 TB for the full archive. 

### How do I run Mana-Ethereum?

We have a command line interface available for syncing a chain from an RPC client (e.g. Infura) or a local client. Information and instructions are available on the [CLI application page](https://github.com/mana-ethereum/mana/tree/master/apps/cli). The basic command is currently run as a mix task:

```mana> mix sync --chain ropsten```

### How do I test?

Testing methods including running Ethereum Common tests are located in the project [README](https://github.com/mana-ethereum/mana#Testing).

### Can I use Mana-Ethereum to mine Ether?

Mining is not supported. Our focus is on optimizing day-to-day operations such as reading and creating transactions, connecting to the network, and syncing the blockchain in a fast, efficient manner.  

### Where can I learn more about Ethereum, the yellow paper etc.?

There are many resources available, the best place to start is on the [Ethereum foundation website](https://www.ethereum.org/). Additional resources include:

[Yellow Paper:](https://github.com/ethereum/yellowpaper) note that you can select different versions to see the specifications for each hard fork. This is useful when developing the protocol.
[Ethereum Reading List:](https://github.com/Scanate/EthList) a community curated list of resources.

## How can I contribute to the project?

Please see our [CONTRIBUTING](https://github.com/mana-ethereum/mana/blob/master/CONTRIBUTING.md) page for contribution protocol and instructions. We recommend reading through the [project issues](https://github.com/mana-ethereum/mana/issues) to learn about our current needs and direction.
