# Introducing Rootstock for Ethereum Developers
Bitcoin and Ethereum are the two leading blockchains in terms of widespread adoption and transaction volume. Familiarity with both puts you in a great position to build any kind of decentralized application to cater for most global audiences.

Although Ethereum was built to handle extra complex functionality such as smart contracts from the start, bitcoin was not. Projects like rootstock came to bridge that gap, and are doing a pretty good job at it.
<br/>Rootstock is part of the bitcoin ecosystem and functions as a bitcoin sidechain. A sidechain operates as an independent blockchain, not to be confused with a layer 2 scaling solution. While L2's like base, optimism, etc. work to submit transactions to the underlying L1(ethereum), they are not blockchains on their own and their transactions have to be ultimately settled in the underlying network.
<br/>Ethereum is a standalone blockchain with big differences from bitcoin yet several key similarities with Rootstock.


## Mining
Mining on ethereum is quite straightforward - validators stake ETH and contribute blocks to the network(Proof of stake). In case the validator wanted to mine blocks for another chain, they would need to follow its different rules either for proof of work or proof of stake and cover the extra computation overhead.

On rootstock, [merged mining](https://blog.rootstock.io/noticia/rsk-bitcoin-merge-mining-is-here-to-stay/) is used. Merged mining is a method of mining blocks for several blockchains simultaneously. The main point of merged mining is that rootstock blocks are linked by bitcoin miners to bitcoin blocks, which greatly enhances security by maintaining a second reference in case malicious actors want to add invalid blocks to the network.
<br/>About half the bitcoin miners perform merged mining on rootstock and they are compensated for it on block confirmation through a smart contract on rootstock. Contrary to ethereum, rootstock uses a Proof of Work mechanism. 


## Smart contract compatibility
Unlike its primary chain bitcoin, Rootstock was built from the ground up with smart contracts in mind. Contracts are executed by the Rootstock Virtual Machine(RVM), which can support any language compiled by the Ethereum Virtual Machine (EVM). This makes it easy to use the same tools for smart contract development that you would use on ethereum such as hardhat and foundry.  Libraries such as openzeppelin are fully supported.
<br/>Check out [these development guides](https://dev.rootstock.io/dev-tools/dev-environments/) to get started.

## Native Currency
The native currency used in Rootstock is Rootstock Smart Bitcoin (RBTC), which is pegged 1:1 to BTC (1 BTC=1 RBTC).
<br/>To obtain RBTC, a user sends bitcoin from the origin network to a special address called the Powpeg. The bitcoin is locked and an equivalent amount is received on rootstock (peg in).
The reverse transaction is also possible (peg out), where the RBTC is sent to the Powpeg address and converted back to bitcoin to the specified address in the bitcoin network.

RBTC is used to pay for transaction fees in the network, which is in turn used to pay miners for executing transactions.


## Supported Tokens
Just like ethereum, rootstock supports various token types- including erc20(fungible tokens), erc721(NFTs) and erc1155(fungible and non-fungible tokens) due to great smart contract compatibility.

Unlike ethereum, rootstock supports [runes](https://dev.rootstock.io/resources/guides/runes-rootstock/overview/) due to integration with bitcoin. Runes can be thought of as a fully on-chain alternative version of ethereum's erc20 tokens, but with no smart contracts involved. A user creates a new rune (etching), and its data is stored fully on the bitcoin blockchain. These can be transferred to other accounts via transactions and are reliant on bitcoin's [UTXO](https://www.investopedia.com/terms/u/utxo.asp) model.

Opinions remain divided regarding the adoption of runes, but just in case you would like to stay ahead of the curve, Rootstock already has infrastructure set up to handle transfer and distribution of runes through the [mock bridge](https://dev.rootstock.io/resources/guides/runes-rootstock/build-mockbridge-contract/Intro-runes/).

## Name Service
Nobody likes to write or remember long blockchain addresses. Ethereum already has a solution for this (ENS). Rootstock has a similar service, the [RIF Name Service](https://rif.technology/content-hub/rif-naming-service/) which gives human readable names to your public address. 

## Ecosystem
Rootstock has enabled the rise of new types of applications in the bitcoin ecosystem due to the speed and scalability achieved so far, the most popular ones centered around decentralized finance.
- [Sushiswap](https://www.sushi.com/)
- [Money on Chain](https://moneyonchain.com/)
- [Sovryn](https://sovryn.app/)
  
These are just but a few examples of the potential surrounding the bitcoin and rootstock space. A more comprehensive list can be found [here](https://rootstock.io/ecosystem/).


## Conclusion
The total supply of bitcoin is about $1.6 Trillion - this is practically begging for innovation in various sectors in the ecosystem. Full ethereum compatibility means that you can port over the same app on Ethereum to Rootstock with minimal configuration and network fees way cheaper than both the ethereum and bitcoin mainnets.
<br/>I highly encourage ethereum users and dapp developers to participate in rootstock as well, the efforts made for compatibility and ease of use are quite worth it.




