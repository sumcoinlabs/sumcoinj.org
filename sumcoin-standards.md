---
layout: base
title: "Which BIPs are supported by sumcoinj"
---

# Which BIPs are supported by sumcoinj

## Introduction

_Sumcoin improvement proposals_ are the communities way of standardising new extensions and protocols that build on top of Sumcoin. Sumcoinj either implements or provides API's to help you implement many of these standards; below you can see which BIP's are supported:

<table>
<thead>
<tr class="header">
<th align="left">Number</th>
<th align="left">Name</th>
<th align="left">Relevant API</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">BIP 11</td>
<td align="left">m-of-n multisig transactions</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/script/ScriptBuilder.html">ScriptBuilder</a></td>
</tr>
<tr class="even">
<td align="left">BIP 14</td>
<td align="left">Protocol version and user agent</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/PeerGroup.html#setUserAgent-java.lang.String-java.lang.String-java.lang.String-">PeerGroup.setUserAgent()</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 16</td>
<td align="left">Pay to script hash (P2SH)</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/LegacyAddress.html">LegacyAddress</a></td>
</tr>
<tr class="even">
<td align="left">BIP 21</td>
<td align="left">Sumcoin URI scheme</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/uri/SumcoinURI.html">SumcoinURI</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 31</td>
<td align="left">Ping/pong messages</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/Peer.html#ping--">Peer.ping()</a></td>
</tr>
<tr class="even">
<td align="left">BIP 32</td>
<td align="left">HD wallets</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/wallet/DeterministicKeyChain.html">DeterministicKeyChain</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 35</td>
<td align="left">mempool message</td>
<td align="left">used automatically</td>
</tr>
<tr class="even">
<td align="left">BIP 37</td>
<td align="left">Bloom filtering</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/PeerFilterProvider.html">PeerFilterProvider</a> (used automatically)</td>
</tr>
<tr class="odd">
<td align="left">BIP 39</td>
<td align="left">Mnemonic codes for representing private keys</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/crypto/MnemonicCode.html">MnemonicCode</a></td>
</tr>
<tr class="even">
<td align="left">BIPs 70,&nbsp;71,&nbsp;72</td>
<td align="left">Payment protocol</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/protocols/payments/PaymentSession.html">PaymentSession</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 38</td>
<td align="left">Encrypted private key serialization</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/crypto/BIP38PrivateKey.html">BIP38PrivateKey</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 111</td>
<td align="left">NODE_BLOOM service bit</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/VersionMessage.html#NODE_BLOOM">VersionMessage.NODE_BLOOM</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 130</td>
<td align="left">sendheaders message</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/SendHeadersMessage.html">SendHeadersMessage</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 141</td>
<td align="left">Segregated Witness (Consensus layer)</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/TransactionWitness.html">TransactionWitness</a>, <a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/Transaction.html">Transaction</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 143</td>
<td align="left">Transaction Signature Verification for Version 0 Witness Program</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/Transaction.html#hashForWitnessSignature-int-org.sumcoinj.script.Script-org.sumcoinj.core.Coin-org.sumcoinj.core.Transaction.SigHash-boolean-">Transaction.hashForWitnessSignature()</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 144</td>
<td align="left">Segregated Witness (Peer Services)</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/Peer.html">Peer</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 159</td>
<td align="left">NODE_NETWORK_LIMITED service bit</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/VersionMessage.html#NODE_NETWORK_LIMITED">VersionMessage.NODE_NETWORK_LIMITED</a></td>
</tr>
<tr class="odd">
<td align="left">BIP 173</td>
<td align="left">Base32 address format for native v0-16 witness outputs</td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/Bech32.html">Bech32</a>, <a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/SegwitAddress.html">SegwitAddress</a></td>
</tr>
<tr class="odd">
<td align="left">RFC 6979</td>
<td align="left"><a href="https://tools.ietf.org/html/rfc6979">Deterministic usage of ECDSA</a></td>
<td align="left"><a href="https://sumcoinj.github.io/javadoc/0.15/org/sumcoinj/core/ECKey.html">ECKey</a></td>
</tr>
</tbody>
</table>
