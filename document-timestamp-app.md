---
layout: base
title: "Building a document timestamping contracts app in under 30 minutes"
---

<div markdown="1" id="toc" class="toc"><div markdown="1">

* Table of contents
{:toc}

</div></div>

<div markdown="1" class="toccontent">

# Tutorial: Building a document timestamping contracts app in under 30 minutes

(Unfortunately, the video has been removed by the creator.)

Document timestamping is the process of proving that a document existed at a certain time, usually, without revealing the contents of that document to the entity doing the timestamping. Examples include financial reports, predictions about the future, invoices and so on.

By inserting a hash into the block chain using a special transaction output type, called an OP_RETURN output, we can build a timestamping app on top of Sumcoin. This video tutorial shows how.

The program saves proof files with a .timestamp extension: these proofs contain what's called a Merkle branch. You can learn more about Merkle trees and branches at the <a href="https://bitcoin.org/en/developer-guide#transaction-data">official Sumcoin block chain developer guide</a>.

## Overview

The tutorial covers:

1. How to start from the wallet-template app
2. How to build a complex, multi-step block chain proof
3. How to bind block chain state to the GUI

## Get the code

You can find the code on github here:

<a href="https://github.com/mikehearn/devcoretalk">https://github.com/mikehearn/devcoretalk</a>

</div>