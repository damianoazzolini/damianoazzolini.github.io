---
layout: post
title: Blockchain Interoperability Solutions
date: 2019-06-09
categories: blockchain
---

One of the main issues of a blockchain system is scalability. 
The impossibility to sign and validate transactions at high rate, 
make this system still unsuitable for several types of tasks, such
as real time payments. 
Considering Bitcoin, its consensus algorithm, proof of work (PoW),
requires to solve a computationally intensive problem which requires
approximately 10 minutes (the difficulty of the problem is dynamically
changed in order to maintain an average of 10 minutes block generation).
Moreover, the block size limit is set to approximately 1 Mb thus making 
the Bitcoin supporting less than 10 transactions per second.
Several solution have been proposed to improve the speed of the network, 
one of them is to increase the block size limit. However, this is a 
[very discussed topic](https://en.bitcoin.it/wiki/Block_size_limit_controversy)
and it is very unlikely that the block size will change anytime soon.
One of the main reason is because, increasing the block size, the block
itself will propagate slower in the network increasing the number of
forks. The block size discussion drove to a hard fork in the Bitcoin Cash
community with the birth of Bitcoin Cash SV.

## Bitcoin Solutions

Several solution have been proposed to increase the blockchain scalability.
Starting from Bitcoin, the solution of 
[Pegged Sidechain](https://blockstream.com/sidechains.pdf)
was one of the first proposal. The main idea is to generate a parallel chain 
(or more than one) where bitcoin can be transferred an spent in a fast way
using a *two-way peg*. 

Another proposal for Bitcoin was [Lightning Network](https://lightning.network/),
which nowadays is available to use. The main idea in this case is to create the
so called (micro) *payment channels* where two or more users can lock funds (bitcoin)
and transact in a fast way. Two users can open a channel that can be connected to 
another channel which can be used by a person that not necessarily trusts the 
creators of the channel. This is possible thanks to 
[*multi signature*](https://en.bitcoin.it/wiki/Multisignature) and 
[*timelock*](https://en.bitcoin.it/wiki/Timelock).

## Ethereum Solutions

Also Ethereum has some proposals to increase the scalability:
- [Plasma Cash](https://hackernoon:com/eli5-plasma-cash-ff242c55e8de), an implementation
of [Plasma](https://plasma.io/plasma.pdf) focused on token transfers.
- [Loom](https://loomx:io/) which proposes the creation of *DAppChains*, sidechains that
allows the execution of dapps (Distributed Applications). The basic idea is to have a 
chain for each dapp and then, before interacting with the dapp, tokens are transferred 
from the main chain to the dappchain where they can be spent and used in a quick way. 
When the interaction finishes, the tokens will be retransferred to the main chain.


## General Solutions

There are also several projects that does not involve Bitcoin. Just to name a few:
- [Cosmos](https://github.com/cosmos/cosmos/blob/master/WHITEPAPER.md) which goal
is to crate a network of blockchain interconnected in a *hub and spoke* model and 
based on [Tendermint Core](https://tendermint.com/docs/). In this network, tokens 
can be transferred from a blockchain to another one using *coin packets* routing 
through the hub.
- [Polkadot](https://github.com/polkadot-io/polkadot-white-paper), defined in the
whitepaper as a *scalable heterogeneous multichain* composed by several actors that
cooperate in order to maintain the network. There are several roles such as *nominator*,
*validator*, *collator* and *fisherman*. Moreover, once the system will be completed,
it will be possible (according to the whitepaper), to connect also Bitcoin and Ethereum
through a so called *bridge*.

## Conclusions

To conclude, in this article we briefly reviewed a list of existing solutions to 
improve the performances of a blockchain system. There are several other existing 
proposals but here we list only the most famous one. However, at the moment, there is 
not a clear winner and the solutions still need improvements.
