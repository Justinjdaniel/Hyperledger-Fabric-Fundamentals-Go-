# Hyperledger Fabric Knowledge Base Refresh

This document provides an overview of the key concepts and components of Hyperledger Fabric, a popular open-source blockchain framework. It is designed to help refresh your understanding of Fabric's architecture, core features, and terminology

## Introduction to Hyperledger Fabric
Hyperledger Fabric is a modular blockchain platform that enables the development of permissioned networks. It offers a flexible and scalable approach to building decentralized applications (dApps) across various industries, such as finance, supply chain, and healthcare.

## Key Components of Hyperledger Fabric
Hyperledger Fabric consists of several key components that work together to create a secure and efficient blockchain network:
- **Peers**: Peers are the fundamental building blocks of a Fabric network. They host ledgers and smart contracts (called chaincode) and communicate with other components to ensure data consistency and integrity.
- **Orderer**: The orderer is responsible for ordering transactions into blocks and distributing them to peers. It plays a crucial role in maintaining the consistency of the ledger across the network.
- **Channels**: Channels enable the creation of private subnets within the network, allowing for confidential transactions and data sharing among authorized participants.
- **Chaincode**: Chaincode is the term used in Fabric for smart contracts. It defines the rules and logic that govern the transactions within the network.
- **Ledger**: The ledger is a tamper-evident record of all the transactions that have occurred within the network. It consists of a blockchain (which stores the transaction history) and a state database (which stores the current state of the network).

## Fabric Network Topology
Fabric networks can be configured in various ways to suit different use cases and requirements. The most common topologies include:
- **Single Organization**: A network with a single organization, suitable for testing and development purposes.
- **Multiple Organizations**: A network with multiple organizations, each with its own peers and orderers, enabling collaboration and data sharing among different entities.
- **Consortium**: A network managed by multiple organizations, with a shared governance model and a common set of rules and policies.

## Fabric Transactions and Consensus
Fabric uses a unique consensus mechanism called Pluggable Consensus, which allows for the implementation of different consensus algorithms based on the specific requirements of the network. The most commonly used consensus algorithm in Fabric is Raft, which provides crash fault tolerance and supports horizontal scaling.

## Fabric Identity and Access Management
Fabric provides a robust identity and access management system based on X.509 certificates. Each participant in the network is identified by a unique certificate, and access control policies are defined to govern the actions that each participant can perform.

## Fabric Security and Privacy
Fabric offers several security and privacy features, including:
- **Confidential Transactions**: Fabric supports confidential transactions, allowing for the encryption of transaction data and ensuring that only authorized participants can access the information.
- **Private Data Collections**: Private data collections enable the storage of sensitive data off-chain, with only the hash of the data stored on the blockchain for verification purposes.
- **Endorsement Policies**: Endorsement policies define the set of peers that must endorse a transaction before it can be committed to the ledger, ensuring that transactions are validated by the appropriate parties.

## Shared Ledger and Shared Progrmas

The shared ledger in HLF offers two promising components:
1. Wordl State
2. Transaction Log

World State in Hyperledger Fabric holds the current values of a key in a business object, or simply it describes the latest state of the ledger. Transaction Log is a record of all transactions which result in changing the values of the world state.

By default, the world state implementation in Hyperledger Fabric is done using **LevelDB** (Key-value data store). Hyperledger Fabric has a pluggable architecture. If needed, Fabric allows you to plug **CouchDB** for the world state storage. CouchDB helps you to perform much more sophisticated queries.

LevelDB is a persistent key-value store library, and CouchDB is an open-source NoSQL document database that collects and stores data in JSON-based document formats.

Hyperledger Fabric boasts a unique feature called **Channels** which allows participants in the network to make discreet communication thereby enabling private communication between the participants in the channel.

Hyperledger Fabric also offers another important feature called **Private Data Collections (PDC)**, which allows a defined subset of organizations on a channel to do transactions without having to create a separate channel.

The process of achieving this agreement is termed as **consensus**. A consensus protocol is essential for DLT(Distributed Ledger Technology) on how to process and update a shared database.

In Hyperledger Fabric, currently used consensus protocol for development and production scenarios is **RAFT**.

## Components in HLF

**Peers**: Peers host ledger instances and smart contracts. All the data related to the organization are stored in the respective peer instances. And also, the smart contracts which consist of all the business-related functions will be installed in the peer instance itself. A peer can serve as a committing peer or endorsing peer. When a peer serves as endorsing peer, it will endorse the transaction proposal from the client, and when as committing peer, it will commit the block to its blockchain.

And there are two types of peers present in the Fabric network

- **Leader Peer**: Leader Peer is responsible for communication between the peers in an organisation.
- **Anchor peer**: Anchor Peer is used to perform cross-organisation communication.

**Channels**: Channel enables communication between all the peers, orderer and applications. It is also possible to create multiple channels in a network with the same or different participants. Each channel will have its own rules and help the participants to transact privately.

**Chaincode**: Chaincode is a package that contains smart contracts designed for organizations based on business requirements. It will be hosted in the peer.

**Orderer**: An orderer is a collection of nodes that orders the transactions into blocks and broadcasts them to the peers. It is considered the heart of the network since it creates the blocks and distributes them to the peers to update the ledger. There are three types of ordering services: Solo, Kafka, and Raft. 

**Client**: Applications are used to connect the peers and execute the transactions. They can be built using the SDKs available. Application is mainly used for ledger updating and querying. It will do it by calling the functions in the chaincode defined in peers.

**Ledger**: It is the place where the actual data of the business assets are stored. The client will fetch or write data on the ledger using CRUD operations. All the historical values of assets are stored here. The ledger is hosted on a peer.

## Transaction Flow

The transaction will be passing through three stages:

1. Transaction Proposal Generation
2. Endorsement
3. Block Distribution

![transaction flow](assets/TransactionFlow.gif)


**Step 1**: The client generates a transaction proposal message. The proposal contains ClientID, chaincode, and transaction payload. The client signs the proposal message with a private key.

**Step 2**: The Client Node / Application Node submits the transaction proposal to the Endorsing peer for approval. The Endorsement is done based on how the Endorsing policy is defined.  Based on the policy, the proposal is submitted to Endorsing peers.

> [!NOTE]
> Endorsing is the process of setting rules among the organizations, for a transaction. The rules are set using an expression called an endorsement policy.

**Step 3**: After performing the Endorsement logic, the Endorsing Peer may either sign or reject the proposal. The signing of the proposal results in the creation of Transaction Proposal Response which can be called an Endorsed message.

**Step 4**: The client verifies the signature of the endorsing peers by comparing the proposal responses. If everything is fine, then the client sends the transaction to the orderer.

**Step 5**: The Orderer receives the transactions in a group and orders them according to the sequence it received. It then creates a block of transactions based on parameters like block size or block timeout whichever is earlier. After it creates a block, it broadcasts it to all the peers in the channel.

**Step 6**: Peer performs its own validation on the blocks received from the orderer. If the validation is successful, then the data in the block is added to the ledger marked as “Valid”. If unsuccessful, it is marked as “FAILURE” and added to the transaction log.

**Step 7**: Finally, the client can be notified that the transaction is completed by using events. The client records it as a successful or unsuccessful transaction.
