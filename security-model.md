---
layout: base
title: "Understanding the sumcoinj security model"
---

<div markdown="1" id="toc" class="toc"><div markdown="1">

* Table of contents
{:toc}

</div></div>

<div markdown="1" class="toccontent">

# Understanding the sumcoinj security model

_Learn about the difference between full vs simplified modes, and how a sumcoinj app can be attacked._

## Introduction

sumcoinj supports two different modes for your application: full verification and simplified verification. The mode you choose controls the resource usage of your application and how much trust you need in other participants in the Sumcoin system. As a developer, it's important you understand the differences and in which situations your app can or cannot be trusted.

Firstly, let's recap how a regular full node works. The fundamental problem Sumcoin solves is achieving consensus on who owns what. Every node maintains a database of unspent outputs, and transactions that attempt to spend outputs that don't exist or were already spent are ignored. Blocks are solved by miners and broadcast to ensure everyone agrees on the ordering of transactions, and so nodes that don't see a broadcast transaction for some reason (eg, they were offline at the time) can catch up.

The act of checking, storing and updating the database for every single transaction is quite intensive. Catching up to the current state of the database from scratch is also very slow. For this reason, not every computer can run a full node.

sumcoinj implements both full mode and _simplified payment verification_. In this mode, only transactions that are relevant to the wallet are stored. Every other transaction is thrown away or simply never downloaded to start with. The block chain is still used and broadcast transactions are still received, but those transactions are not and cannot be checked to ensure they are valid. This mode of operation is fast and lightweight enough to be run on a smartphone, but can be defeated in various ways.

## Pending transactions

When a transaction is broadcast over the network we say it is _pending_ inclusion in a block. Mining nodes will see the transaction, check it for themselves and if it's valid, include it in the current block they're trying to solve. Nodes do not relay invalid transactions.

Your app will receive pending transactions, add them to the wallet, and run event listeners. However, in SPV mode the only reason you have to believe the transaction is valid is the fact that the nodes you connected to relayed the transaction. If an attacker could ensure you were connected to his nodes, this would mean they could feed you a transaction that was completely invalid (spent non-existing money), and it would still be accepted as if it was valid. 

Because sumcoinj apps do not accept incoming connections, the peers you talk to are always randomly selected at startup (based on DNS seeds today). So it can be difficult for an attacker to control your connectivity like that. For this reason, the number of peers that have announced a transaction is exposed in the `TransactionConfidence` object and you can listen on that to learn when new peers have announced a transaction. Once most of your peers have announced, you can be fairly sure that the transaction is propagating its way across the network and is very likely to be valid. 

There are three potential attacks on this method of gaining confidence.

1. Hijacking your entire internet connection and connecting you to a fake network. This is called a _Sybil attack_ and is easiest to pull off when you are using an untrusted internet connection (e.g. coffee shop wifi), or using Tor. Sumcoinj does not support Tor today, so realistically, this concern is biggest when using a mobile wallet.
2. Exploiting race conditions by broadcasting two invalid transactions simultaneously. This technique was explored in [a paper by researchers at ETH Zurich](http://eprint.iacr.org/2012/248.pdf). For the technique to work best the attacker must be able to connect to the victim. sumcoinj apps do not accept incoming connections (there is no reason for them to do so), so this is difficult to pull off. In future the Sumcoin network will likely relay double spend alerts, but it's not implemented today.
3. Mining a block that contains a double spend, then buying a service, then broadcasting the block. This is known as a _Finney attack_ and is discussed below.

## Finney attacks

In a Finney attack the attacker mines a block including a spend of some of his coins to another address controlled by him. Once he finds a block, he does not broadcast it immediately. Instead he goes to a merchant who is accepting unconfirmed transactions and spends the coins. Once he obtained the goods he wanted from the merchant, he broadcasts his block containing the double spend, and takes back the coins. The Finney attack relies on careful timing and a lot of patience by the attacker: he must wait until he has found a block, which can take a long time. He must be able to buy something from a merchant quickly - every second he spends waiting for the goods to be delivered is a second another miner may discover and broadcast a valid block, making his work worthless.

If you meet these criteria you may be susceptible to a Finney attack:

1. You are irreversibly selling something of value in return for pending transactions
2. The attacker can choose the time of purchase
3. The process of purchasing is relatively fast (less than a few minutes)

Here are some examples of merchants that are, or are not, susceptible to the attack:

* An automated online store that sells video game downloads and wants to make the download available immediately, without waiting for confirmations. Because it's an online store that's open 24/7 the attacker can choose when to perform the purchase. Because it's automated the purchasing process is fast. Because it's a download the sale is irreversible unless you can revoke some kind of online licensing check. **Susceptible**
* A supermarket. The sale is irreversible once you walk out of the store. However, the attacker does not control the exact time of purchase. It isn't feasible to camp inside a supermarket waiting for a block solution to be found unless you have a significant fraction of the networks total mining power and are finding blocks every hour. Even if you were to find a block like that, the time of purchase still varies depending on the queues at the checkout counter. Once a block is found every second counts and you may fail the attack with greater probability the longer you wait, so time taken to purchase matters. **Not susceptible**
* An in person currency trade. There is a high value irreversible transaction taking place. But again, you can't typically organize and pull off an in-person trade in an instant fashion, it requires more organization ahead of time. Even though you can suggest a time to meet up with the other person and do the trade, the "process of purchasing" takes a long time. **Not susceptible**

If somebody executes a Finney attack against your app, the `TransactionConfidence` confidence type for that transaction will change to `DEAD` and any event listeners you registered will be called. `DEAD` transactions should be treated as if the payment has been reversed, and will not be counted towards your balance.

## Confidence of confirmed transactions

Many types of application don't deliver a service immediately, and there it's OK to wait for confirmation of transactions via inclusion in the block chain. When a transaction is included into the chain, the `TransactionConfidence` type changes to BUILDING and you can then access the transactions _depth_ (how many blocks have been built on top of this transaction), and _work done_, which is another view of the same thing. For example, immediately after a transaction has appeared in a block its depth is 1 and the work done depends on current [network speeds](http://bitcoin.sipa.be/). After an hour, on average there should be 6 blocks though the actual amount many vary considerably. The transaction confidence listeners run every time a new block is received, so you can register a listener and use that to trigger the act of delivering your goods or services when a confidence level is reached.

**Recall that confidence can go down as well as up**. A "reorganize" is what happens when the chain you are on is replaced by a new best chain that ran parallel to yours. A re-organize can change your wallet arbitrarily, eg, by making transactions that were previously confirmed unconfirmed or (in the case of a double spend) dead. Re-orgs that make a transaction go dead are discussed in Satoshis paper, and are slightly different to the Finney attack case where there is no re-org, just a new best block. It's very rare for re-orgs to change the confidence of transactions that are buried deep in the chain.

Just because a transaction appears in a block does not mean it is valid. Again, sumcoinj apps do not check transaction validity. Instead the assumption is that it's difficult to build a block chain containing an invalid block because you would have to be able to outrun the rest of the miners combined. There is one exploit against this assumption that is not yet fixed: **if an attacker can control your connections to the Sumcoin network, they can prevent you from seeing newly found legitimate blocks and mine their own invalid block containing a bad transaction**. This attack is detectable because unless the attacker can outrun the network (a 51% attack), the speed with which new blocks arrive will drop significantly. In future sumcoinj may offer a "red alert" mode in which things that seem odd are flagged, it would indicate to your app that it's time to stop trading.

If you are delivering something of high value, how much confidence should you require? The traditional "rule of thumb" is six blocks, or an hour. An alternative way to look at this is to figure out how long you can realistically wait, then look at how much work is done on average in that timespan, and then require that much work done. This insulates you from varying interest in mining over time and ensures the cost of doing a double spend attack is the same.

## Proving inclusion in a block without the full block body

To find transactions relevant to your wallet, we have two options. We can download the full block contents and scan all transactions. This is inefficient - much data is downloaded only to be thrown away. Or, we can request transactions that match a pattern from remote nodes. We do this using Bloom filters when the remote node supports them (v0.8 and up). This leads to the question of how you can know the received transaction really did appear in the block chain, if you don't have a full copy of the block.

Blocks contain a list of transactions, one after the other. From this list, a _Merkle tree_ is calculated. This structure generates a _Merkle root_, a single hash value that is then placed in the block header. The approach is more complex than the obvious one of simply hashing the concatenation of the transactions, but it has a major advantage: it's possible to prove a transaction was in a block by providing only that transaction and a _Merkle branch_. The branch consists of hashes making up sibling nodes in the original tree. If a node hands you a block header, a transaction and a branch you can check for yourself that the transaction was indeed accepted by the network and is unlikely to have been forged. The branch takes up much less space than the full block body, so this is a major efficiency win. And multiple transactions can have their merkle branches combined together for even greater efficiency.

</div>
