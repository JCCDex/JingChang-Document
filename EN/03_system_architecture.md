# System architecture !heading

The blockchain in the financial industry is not a public chain and must meet financial regulatory requirements. Therefore, this requirement must be fully considered in the system architecture. This chapter must meet the matters that should be noted in the blockchain system architecture of financial services.

## System architecture overview

![local image](../Images/02_system_architecture.png)

* Zone 1：Blockchain network is also a virtual network running on the Internet.
* Zone 2/3/4：The node zone in the blockchain network can be a single node or a mixed zone of nodes and business applications;
  * DMZ：Dmilitarized zone, is located between two firewalls.It is usually recommended to deploy blockchain node services in this zone to connect the blockchain network and the internal business network;
  * SEAAPS: SEAAPS is applications which provides basic services for blockchain, such as blockchain browser, oracle service(Not a database service, but coordination of on-chain and off-chain data services);
  * Intranet/Service: Business application services are banks' own internal business systems, such as bank account systems, KYC services, etc.
  * Regulator: Financial industry regulators to supervise transactions to meet anti-money laundering (AML) management needs;
  * TAX: Tax regulator；
* Triangle image：The consensus node representing the blockchain, usually the accounting node designated by the alliance chain management organization for election. Consensus nodes are responsible for voting and accounting.
* Circle image: access(public) nodes, nodes that the alliance chain allows access to, nodes without accounting rights

## Node

The SWTC blockchain is a virtual network connected by nodes of multiple participants. Each participant in the network can have one or more nodes. They exist in the form of network services, and each node maintains all Or part of the ledger data, there are multiple copies of the ledger data in the entire blockchain network, so single or part of the nodes will be disabled, no data will be lost, and the service will not be stopped.

The consensus protocol of SWTC Public Chain adopts randomized BFT. The core part of SWTC Public Chain contains several validating nodes that maintain the underlying validating network for the system. This network opens for every application on SWTC Public Chain access. DAPPs on SWTC Public Chain refer to the applications based on SWTC Public Chain for specific users. These applications can directly access to the public SWTC Public Chain through the API provided by the SWTC Public Chain. These applications can help to validate.

Two functions may be implemented:

1. it is involved in the consensus of public nodes in the network on the SWTC Public
Chain.
2. if an application only uses aPi to access to the blockchain, there is no need to deploy a single authentication node.

The RBFT algorithm has a one-third fault tolerance rate, and the number of verified nodes is recommended to be no less than 6 nodes.

## Consensus algorithm

The SWTC Public Chain technology uses a self-owned proprietary BFT consensus algorithm.

Under RBFT mechanism, there is a concept called view. in a view, there will be primary node (replica), and the rest of the nodes are called backups. The primary node is responsible for ordering the requests from the client and then sending them to the backup nodes in order. This primary node of RBFT has more rights than other nodes, and if it has a problem, it will cause a relatively large delay in the system. in rBFT, this point has been improved. referring to the mechanism of election in raFT, voting is adopted, and there is no need to snatch the accounting right to ensure the fairness of the rights of each node.

## Financial blockchain

The financial blockchain is an alliance chain and a permission chain.

The financial blockchain is a network of companies in the financial industry. Through this network, various financial services are run. Only transaction members in the alliance are eligible to view it. Therefore, members of the alliance need to be approved to access the alliance chain. .

The regulatory authority (including the tax authority of the host country), as one of the node members, monitors transactions in real time to meet anti-money laundering regulations and other business regulatory requirements. In the event of a violation of the regulation, the relevant transaction wallet can be implemented through the alliance chain's governance mechanism. Freezing to prevent greater illegal consequences.
