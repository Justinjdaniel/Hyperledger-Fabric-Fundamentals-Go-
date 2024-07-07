# Refresh Memory 

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

**Peers**:

Peers host ledger instances and smart contracts. All the data related to the organization are stored in the respective peer instances. And also, the smart contracts which consist of all the business-related functions will be installed in the peer instance itself. A peer can serve as a committing peer or endorsing peer. When a peer serves as endorsing peer, it will endorse the transaction proposal from the client, and when as committing peer, it will commit the block to its blockchain.

And there are two types of peers present in the Fabric network

- Leader Peer: Leader Peer is responsible for communication between the peers in an organisation.
- Anchor peer: Anchor Peer is used to perform cross-organisation communication.

**Channels**:

Channel enables communication between all the peers, orderer and applications. It is also possible to create multiple channels in a network with the same or different participants. Each channel will have its own rules and help the participants to transact privately.

**Chaincode**:

Chaincode is a package that contains smart contracts designed for organizations based on business requirements. It will be hosted in the peer.

**Orderer**:

An orderer is a collection of nodes that orders the transactions into blocks and broadcasts them to the peers. It is considered the heart of the network since it creates the blocks and distributes them to the peers to update the ledger. There are three types of ordering services: Solo, Kafka, and Raft. 

**Client**:

Applications are used to connect the peers and execute the transactions. They can be built using the SDKs available. Application is mainly used for ledger updating and querying. It will do it by calling the functions in the chaincode defined in peers.

**Ledger**:

It is the place where the actual data of the business assets are stored. The client will fetch or write data on the ledger using CRUD operations. All the historical values of assets are stored here. The ledger is hosted on a peer.