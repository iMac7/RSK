# Union Bridge - The Powpeg Successor?
Bitcoin is the largest blockchain network, and arguably the most secure due to its Proof of Work mechanism. However, part of the security comes down to being limited in terms of the computations that can be performed on-chain.
<br/>The biggest challenge yet when building on other chains that depend on bitcoin (Rootstock) is maintaining communication with the primary chain.
Blockchains that were built to be highly programmable from the onset, such as Ethereum, have less of a problem here. In bitcoin, this needs way more innovation to pull off.

A blockchain bridge is a way to transfer assets from one blockchain to another. Powpeg is the longest serving and most secure bridge on bitcoin to date.

## Powpeg
Powpeg is a bridge for transferring bitcoin from the primary bitcoin chain to rootstock.
<br/>In order to receive bitcoins on rootstock (peg in), a user sends their bitcoin to a specific bitcoin address on the bitcoin network where it is locked.
The wallet is actually a multisig wallet controlled by the powpeg federation and each member is called a pegnatory.
The rootstock blockchain monitors the transaction and the same amount of RBTC(Rootstock smart bitcoin) is allocated to the sender on the rootstock network.
<br/>To convert RBTC back to bitcoin (peg out), the user sends the amount of RBTC to convert to a burn address on the rootstock network.
The powpeg federation signs a transaction to send the equivalent amount of bitcoin back to the user's address on the bitcoin network.

### Limitations of Powpeg
Having to rely on members of a multisig wallet implies relying a majority being trustable in the first place (t of N security model - t honest parties, a majority, are needed to secure the network). This introduces an aspect of centralization which goes contrary to the decentralized nature of blockchain.

Pegnatories need to hold a significant amount of collateral to incentivize honesty. This may not be viable for any participant which limits the security of the blockchain.

Bridge transactions (especially peg out) are slow because of requiring a third party to receive and sign transactions on behalf of the user.

Security is dependent on Hardware Security Modules (HSM) in pegnatories being secure themselves. A HSM is a specialized hardware device which stores private keys to prevent access even by the pegnatory itself.

## The search for an alternative
The main function that a bridge depends on is determining that the correct amount of assets go across the bridge.
In the case of powpeg, that the exact amount of btc locked in the primary chain is the same amount of RBTC locked in rootstock and vice versa.
<br/>It was necessary to have a more decentralized way to do this.

### BitVMX
BitVMX is a design for a virtual CPU to execute disputable computations on bitcoin. 
<br/>
Here is a [comprehensive whitepaper](https://www.bitvmx.org/files/bitvmx-whitepaper.pdf) if you would like to dive in to the details, but I will try to (over)simplify it.
<br/>Two parties are involved in the working of bitvmx - a prover and a verifier. A prover generates a proof (e.g.that the said amount was actually deposited) and submits it to the blockchain. 
The prover's funds are locked to incentivize honesty when submitting transactions. A verifier monitors as the prover executes the program again with the expected inputs to confirming a matching output.
Where suspected of cheating, the verifier can challenge the claim by running the same computation herself. In case of proven cheating, the prover's funds are lost and partially distributed to the successful verifier.

### Union Bridge
This is the practical implementation of BitVMX into a new bridge protocol for rootstock. It is meant to be a better version of the long running powpeg as a more efficient alternative.
Read more on the technical details [here](https://arxiv.org/abs/2501.07435).
<br/>For a quick overview, we can look at one case study below.

A user currently on the bitcoin network wants to get RBTC. He sends them to a special address controlled by the Union Bridge.
<br/>Once the bridge receives the funds, there are provers monitoring it ready to start the peg-in process. The prover collects various details from the transaction including transaction id, sender address, amount and confirmations received (prevent double spending). The prover then runs a virtual execution of the transaction in the Bitvmx instance to confirm that the deposit has occurred. This happens off-chain in the prover's computer. The proof generated commits the prover to the transaction and is stored on the bitcoin blockchain. During this process, the prover stakes funds alongside the transaction to ensure they only submit valid proofs.
<br/>Verifiers are independent participants in the blockchain. When a seemingly valid transaction is sent to the blockchain, any verifier can validate the proof in their own Bitvmx instance by running the computation on the combination of transactions and inputs used by the prover. 
<br/>If the proof is invalid, the verifier can submit the proof on-chain and challenge the transaction by using the output to trace through the computation steps used by the prover to identify where exactly the computation failed. A dishonest prover stands to lose part or all of their staked bitcoin once proven on the blockchain.
<br/>If the proof is confirmed to be valid, the prover submits a compressed proof to the rootstock blockchain. It is then recorded by the Union bridge and RBTC is successfully minted to the user's rootstock wallet. The prover is able to withdraw their staked funds after a specified number of block confirmations to ensure enough time for verifiers to challenge. 

<br/>Unlike the powpeg, the union bridge has a 1 of N security model, where N is the number of members in the verifying committee. You only need one honest party to secure the network by challenging invalid proofs, not necessarily a majority.

## Conclusion
The rootstock ecosystem is ever evolving to meet the growing demand on the bitcoin platform. The Union Bridge is an incredible innovation in boosting the security, scalability and decentralization of rootstock and the bitcoin ecosystem as a whole. 




