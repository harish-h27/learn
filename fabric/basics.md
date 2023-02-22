### hyperledger fabric is a distributed ledger technology(DLT) framework for building business blokchain applications

### reasons to use fabric in business

1. permissioned blockchain
2. members(users, peers, admins) identities are known
3. participants have role based access
4. confidential transactions
5. no cryptocurrency
6. programmable(smart contract)


### architecture of fabric

### orderering service

#### orderer
* responsible for creating a block
* provide consensus mechanism
* orderer is implemented with message oriented middleware like SOLO, RAFT, KAFKA

#### peer
- each peer ledger, chaincode and has a gRPC service to connect to client other peers and orderer
- fabric ledger contains transaction logs and world state
- anchor peer are known outside of the organization
- fabric-client sdk acts on behalf of the end user(admins, users)
- fabric-client creates a transaction and submits to network

#### channel

- fabric can have multipe channel
- transactions are isolated within the channel
- chaincode is deployed to the channel not to the network
- there is special channel called bootstrap channel or system channel which got created when the network is initialized
- org peers can join multiple channel


### consortium 

- consortium is an association of two or more individuals,  companies, organizations or government with objective of achieving a common goal
- consortium decides who can make change to blockchain
- consortium in blockchain will be majority of orgs must agress, org level decisions left to org admin

- fabric supports decentralized administration(instead of single centralized administration) by way of policy
- policies are created by members of the constortium and these policies enforce the rule for changes___
like what changes can be made, who can make those changes and how those changes are made to the network all of these are governed by policies

