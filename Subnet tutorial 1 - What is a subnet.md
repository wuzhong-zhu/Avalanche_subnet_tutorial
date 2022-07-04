# Avalanche subnet tutorial series(1): What is a subnet

![img1](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%201/img1.jpeg?raw=true)

Avalanche is the 4th largest blockchain network with [2.67 billion total value locked (TVL)](https://defillama.com/chains) as of April 2022. The reason more and more developers choose Avalanche is because of its improved throughput and scalability.  

- Performance-wise, Avalanche can process [4500 transactions per second (TPS)](https://support.avax.network/en/articles/4136568-how-many-transactions-per-second-does-avalanche-support) right now.  
- Scalability-wise, [subnet](https://docs.avax.network/build/tutorials/platform/subnets/create-a-subnet/) is the key feature making Avalanche stand out from its peers. 

**Subnet allows Avalanche users to create and run their own blockchain networks**, and it is very flexible. A subnet can be set to private, so its transactions are separated from the primary network. Subnet chains can also be tuned to meet different compliance requirements like [geographical bounds and KYC/AML checks](https://docs.avax.network/subnets/#compliance).

This tutorial series is created to help developers start their journey with Avalanche subnets. For testing purposes, it is based on Avalanche’s Fuji testnet, but the steps are applicable to mainnet too. It is extended from [this article on Avalanche’s official website](https://docs.avax.network/build/tutorials/platform/subnets/create-a-subnet), so many code snippets were reused from there.

This tutorial consists of 5 parts:

1. What is a subnet. <– You are here.
2. Running a local Avalanche node on Fuji testnet.
3. Creating a subnet, then create a blockchain on it.
4. Deploying a smart contract.
5. Indexing subnet with The Graph.
   
This is the first article in the series. It is a collection of essential background bits of knowledge and common doubts that may arise during development. It is optional and independent from other articles, so feel free to skip it if you think you have a foundation in Avalanche and come back in the future if needed. 

---
## The roadmap
![img2](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%201/img2.png?raw=true)

This is a comprehensive roadmap of what this series is about. Please take note that all the sample codes, command line inputs, and system setups are **macOS-based**. Developers using different platforms should adjust accordingly depending on their system.

The summary of this tutorial:

1. Running a node for Avalanche’s Fuji testnet.
2. Making the node a validator node.
3. Creating a subnet.
4. Initializing a blockchain network.
5. Deploying a smart contract on a blockchain network.
6. Using The Graph with a subnet.
7. Building a DApp.

---

## About validator nodes 

Many readers may already know what a node is; Nodes are actually AvalancheGo instances/servers/hosts. Nodes keep the state of the blockchain(s) and offer API endpoints for users to query the blockchain and send transactions.

In general, there are two types of nodes on Avalanche: **validator** nodes and **non-validator nodes**. Users can access Avalanche chains through either of them.

[Non-validator](https://docs.avax.network/nodes/build/run-avalanche-node-manually) nodes do not verify transactions, they are merely an endpoint to access the protocol.

[Validator](https://docs.avax.network/nodes/validate/staking#running-a-validator) nodes are [staked](https://docs.avax.network/learn/platform-overview/staking) with AVAX. They participate in the transaction verification process on Avalanche following the [Snowman consensus](https://docs.chainstack.com/blockchains/avalanche) model with staking. On mainnet, the minimum amount that a validator must stake is 2,000 AVAX, while on the Fuji testnet there is no minimum.

This tutorial starts from scratch and begins with creating your own validator node. There are two main reasons for this:

1. To create a subnet, a [Keystore user](https://docs.avax.network/build/avalanchego-apis/keystore/) is needed; and the username and password are all accessible by the node admin. It is recommended to do this on your own node(it doesn’t have to be a validator node).
2. To start validating a subnet, the participating validator nodes must be configured and restarted to whitelist the subnet with its subnet ID.



---
## What are the X-chain, P-chain, and C-chain? 
There are 3 primary blockchains on Avalanche. Together they are running on the [primary network](https://docs.avax.network/learn/platform-overview/) of Avalanche, and all the nodes on Avalanche have access to these three chains. Validator nodes must always be a part of the primary network, so it is not possible to have a validator node for a subnet only.

Chains are accessed through different [APIs](https://docs.avax.network/build/avalanchego-apis/). Each chain serves different purposes.

- **The P-chain is for platform management**. It handles requests related to the validator, the subnet, and the blockchain. 
- **The C-chain is for contract management**. It is based on EVM; hence its API is almost identical to other EVM protocols. It has both RPC and WebSocket endpoints, and it handles requests for smart contracts. 
- **The X-chain is for asset exchange**. It is Avalanche’s native platform; it is used for creating and trading assets like AVAX and NFTs. 



---
## About the AVAX currency and the wallet
AVAX is the native token on Avalanche, and it can be exchanged and consumed on all three chains, for different purposes though.

On the X-chain, **AVAX is traded as the primary currency**. The transaction fee is fixed to 0.001 AVAX.

On the P-chain, **AVAX is used for validator node staking**. A validator node earns AVAX rewards for validating transactions.

On the C-chain, **AVAX is used in smart contracts or to pay for gas**. The transaction and gas fees are not fixed as opposed to the X-chain. Unlike the X-chain and the P-chain, the C-chain address is Ethereum styled, and starts with 0x.

Subnets, on the other hand, do not exchange AVAX; they issue their own tokens.

AVAX tokens are stored in wallets. Each wallet has 3 addresses, one for each chain.

Users can easily manage them using the [official Avalanche wallet](https://wallet.avax.network/).
![img3](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%201/img3.png?raw=true)

AVAX can be transferred either to: 

- An address on the same chain.
- Different chains in the same wallet.
  
It is **not possible** to transfer AVAX to another address on a different chain. 
![img4](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%201/img4.png?raw=true)


Note that addresses on P-chain **cannot** transfer tokens between each other. Neither can they request a faucet airdrop in testnet. To set up a validator node, AVAX needs to be transferred to the X-chain or C-chain address before moving it to P-chain.



---

## About subnets
There is a common misunderstanding that the subnet is a blockchain. A subnet is actually a **set of validator nodes**. The [official documentation](https://docs.avax.network/build/tutorials/platform/subnets/) states that: 

> *A subnet, or subnetwork, is a dynamic set of validators working together to achieve consensus on the state of a set of blockchains. Each blockchain is validated by exactly one subnet. A subnet can validate many blockchains. A node may be a member of many subnets.*

To elaborate on this: 

- A subnet is not a blockchain, it is a group of validator nodes. It is the foundation of blockchains.
- When a subnet contains no validator, its state - becomes invalidated, and any blockchain on it stops functioning.
- One node can be a part of multiple subnets.
- Subnets can be assigned to many blockchains.
- One blockchain can only be validated by one subnet.

![img5](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%201/img5.png?raw=true)


A very brief summary of how to use a subnet: 

1. A user owns one or more validator nodes on Avalanche’s primary network.
2. The user creates a new subnet. No validator is assigned to the subnet yet. 
3. The primary network gets notified; a new subnet is created. 
4. A validator node is assigned to the subnet.
5. The subnet starts getting validated.
6. A new blockchain is created on the subnet.




---
## About customized blockchain

Avalanche supports 3 distinct types of blockchain virtual machines(VM). 

[AVM](https://docs.avax.network/build/tutorials/platform/subnets/create-avm-blockchain) creates a private X-chain. 

The [Subnet EVM](https://docs.avax.network/build/tutorials/platform/subnets/create-evm-blockchain) creates a private C-chain. 

By modifying the source code in the [ChainVM](https://docs.avax.network/build/tutorials/platform/subnets/create-custom-blockchain) interface, developers can create their own chain. 

---
## Conclusion
Hope this article helps to clarify doubts about Avalanche and subnet. In the next articles, we will dive deeper into creating our own subnet, so make sure you will not miss them.  

Happy hacking.