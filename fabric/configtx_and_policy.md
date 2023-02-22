## configtxgen 
- is a command line utility for managnig configuration artifacts, there are three types of artifacts that you can manage___
   from configtxgen tool they are below
   - genesis block: this the first block of system channel which contains configuration of the newtwork
   - channel tx: used to create a application channel
   - anchor peer tx: anchor peer update transaction




- to use confitxgen tool you should export path for configtx.yaml file using FABRIC_CFG_PATH variable,
 otherwise it will search the current folder if not present then it will throw error


 ### configtx.yaml file has six sections

 1. Organizations: -> contains the list of member organizations in the network
 2. Orderer: -> parameters for orderer setup
 3. Application: -> parameters defining the application configuration 
 4. Channel -> contains channel related parameters
 5. Capabilities -> capabilities is the section that is utilized for binary version management across the network
 6. Profile -> profile section allows the setup of multiple configurations in a single section



## Organizations: -> contains the list of member organizations in the network
 
```yaml
Organizations:
      # & means anchor tag, we are defining Org1 as anchor because we can give reference to it from other section of configtx.yaml file
    - &Org1
        # DefaultOrg defines the organization which is used in the sampleconfig
        # of the fabric.git development environment
        Name: Org1MSP

        # ID to load the MSP definition as
        ID: Org1MSP

        MSPDir: ../organizations/peerOrganizations/org1.example.com/msp

        # Policies defines the set of policies at this level of the config tree
        # For organization policies, their canonical path is usually
        #   /Channel/<Application|Orderer>/<OrgName>/<PolicyName>
        Policies:
            Readers:
                Type: Signature
                Rule: "OR('Org1MSP.admin', 'Org1MSP.peer', 'Org1MSP.client')"
            Writers:
                Type: Signature
                Rule: "OR('Org1MSP.admin', 'Org1MSP.client')"
            Admins:
                Type: Signature
                Rule: "OR('Org1MSP.admin')"
            Endorsement:
                Type: Signature
                Rule: "OR('Org1MSP.peer')"
```


##  Orderer: -> parameters for orderer setup

```yaml
Orderer: &OrdererDefaults # 1

    # Orderer Type: The orderer implementation to start
    OrdererType: etcdraft # 2
    
    # Addresses used to be the list of orderer addresses that clients and peers
    # could connect to.  However, this does not allow clients to associate orderer
    # addresses and orderer organizations which can be useful for things such
    # as TLS validation.  The preferred way to specify orderer addresses is now
    # to include the OrdererEndpoints item in your org definition
    Addresses: # 3
        - orderer.example.com:7050

    EtcdRaft:
        Consenters:
        - Host: orderer.example.com
          Port: 7050
          ClientTLSCert: ../organizations/ordererOrganizations/example.com/orderers/orderer.example.com/tls/server.crt
          ServerTLSCert: ../organizations/ordererOrganizations/example.com/orderers/orderer.example.com/tls/server.crt

    # Batch Timeout: The amount of time to wait before creating a batch
    BatchTimeout: 2s # 4

    # Batch Size: Controls the number of messages batched into a block
    BatchSize:

        # Max Message Count: The maximum number of messages to permit in a batch
        MaxMessageCount: 10 # 5

        # Absolute Max Bytes: The absolute maximum number of bytes allowed for
        # the serialized messages in a batch.
        AbsoluteMaxBytes: 99 MB

        # Preferred Max Bytes: The preferred maximum number of bytes allowed for
        # the serialized messages in a batch. A message larger than the preferred
        # max bytes will result in a batch larger than preferred max bytes.
        PreferredMaxBytes: 512 KB

    # Organizations is the list of orgs which are defined as participants on
    # the orderer side of the network
    Organizations:

    # Policies defines the set of policies at this level of the config tree
    # For Orderer policies, their canonical path is
    #   /Channel/Orderer/<PolicyName>
    Policies:
        Readers:
            Type: ImplicitMeta
            Rule: "ANY Readers"
        Writers:
            Type: ImplicitMeta
            Rule: "ANY Writers"
        Admins:
            Type: ImplicitMeta
            Rule: "MAJORITY Admins"
        # BlockValidation specifies what signatures must be included in the block
        # from the orderer for the peer to validate it.
        BlockValidation:
            Type: ImplicitMeta
            Rule: "ANY Writers"

```

1. orderer section is also defined as anchor element so you can refer to it from other section
2. ordererType can be solo, etcdraft
3. list of orderer addresses, if the ordererType is etcdraft then the list of orderer address can be many instead of one
4. number of seconds to wait before creating each block
5. maximum number of transaction in each block

## Application section
- the application section has a subsection organizations under which there is a list of organizations__
that participate in the application type transactions and their policies 


## channel section
- the channel section defines the default set of parameters for the channel and policies

## profile section

```yaml
################################################################################
#
#   Profile
#
#   - Different configuration profiles may be encoded here to be specified
#   as parameters to the configtxgen tool
#
################################################################################
Profiles:

    TwoOrgsOrdererGenesis:
        <<: *ChannelDefaults
        Orderer:
            <<: *OrdererDefaults
            Organizations:
                - *OrdererOrg 
            Capabilities:
                <<: *OrdererCapabilities
        Consortiums: # we are defining the list of Consortiums
            SampleConsortium: # this is one Consortium
                Organizations: # these orgs are part of this Consortium
                    - *Org1
                    - *Org2
    TwoOrgsChannel:
        Consortium: SampleConsortium # we are just telling this channel uses the SampleConsortium as its Consortium
        <<: *ChannelDefaults
        Application:
            <<: *ApplicationDefaults
            Organizations: # who are part of this channel
                - *Org1
                - *Org2
            Capabilities:
                <<: *ApplicationCapabilities
```

- there can be multiple profiles declared inside configtx.yaml file, each of these profiles are named subsections unde the profile section

- there are two profile in above example one is ***TwoOrgsOrdererGenesis*** and ***TwoOrgsChannel***

- the TwoOrgsOrdererGenesis profile is for creating the genesis block
- the TwoOrgsChannel profile is for creating the channel transaction 
- these profile names are used by configtxgen tool 
- to generate a genesis block, it requires certain configuration elements from the configtx.yaml file
- if you look at the profile section it is referencing to many other section inside configtx.yaml

### generating orderer genesis block 

```sh
   > configtxgen -outputBlock <file name> -profile <profile name> -channelId <channel name>
   > configtxgen -outputBlock ./genesis.block -profile TwoOrgsOrdererGenesis -channelId ordererchannel 
   result of this command is , it generates the file genesis.block file, which is used for launching the orderer
```

### dumping the content of the genesis block in json format

```sh
   > configtxgen -inspectBlock <blockfile name>
   > configtxgen -inspectBlock ./genesis.block
```

### creating the create channel transaction file

```sh
   > configtxgen -outputCreateChannelTx <file name> -profile <profile name> - channelId <channel name>
   > configtxgen -outputCreateChannelTx app.tx -profile TwoOrgsChannel -channelId acmechannel
   this command generates app.tx file which is used by peer binary to create(app.block) application channel
```

### dumping the content of the applicaton channle tx file into json 

```sh
   > configtxgen -inspectChannelCreateTx <blockfile name>
   > configtxgen -inspectChannelCreateTx ./app.tx
```

### generating anchor peer update tx files

```sh
   > configtxgen -outputAnchorPeersUpdate <file name> -profile <profile name> -channelId <channel name> -asOrg <org name>
   > configtxgen -outputAnchorPeersUpdate org1anchors.tx -profile TwoOrgsChannel -channelId acmechannel -asOrg Org1
   - above command generates the file org1anchors.tx file which can only be used by org1 in the acmechannel to update the anchor peers 
```
- the configtxgen tool picks up the anchor peer information from the configtx.yaml file using the -profile and -asOrg
   -profile = AcmeChannel(present in the profile section)
   -asOrg = Org1(present in the organizations section)



### dumping the content of update anchor peer tx to json

```sh
   > configtxgen -inspectChannelCreateTx org1anchors.tx
```


### command to print the information on organizations

```sh
   > configtxgen -printOrg Org1 > org1.json

   the above Org1 is referencing to Org1 in Organizaitons sections
```


## polices - rules and principle

what are policies in HLF ?

- hypereledger fabric policies embodies the rules for Access and updates to the network and channel configurations
- these policies are defined by the consortium in configtx.yaml file then becomes part of genesis block

genesis block controls 
   - what changes
   - who changes it
   - how changes happens



***what changes***
network configaration and channel configuartion

***who changes ***
admin , member

***how changes happen ***
- in case of ANY policy , single org need to sign the transaction(which contains changes)
- in case of Majority policy , multi orgs need to sign the transaction


***policy examples***

- to add a ***peer*** -> policy can be any admin from owner org 
- to update ***orderer system channel*** -> policy can be majority admin from all the org
- to add a new member(org) -> polciy can be any admin from any org
- remove exisiting member(org) -> majority admin from all org




### Rules

- rules are heart and soul of the policy
- rules are expressed as boolean expressions in terms of the principles
- principle refers to the singers role in the member organization, organziation issue identities and these identities are 
   associated with rules or principles
- there are four standard principles that can be assigned to the identity
   -> admin
   -> memeber
   -> client 
   -> peer
-> the rule reference is made to the principle by using notation MSPID
ex:
   org1MSP.admin -> this references to admin principle in org1 organization


### relationship b/w identities and principles using example

we have acme organization which has acme CA to issue certificates , it has issued to certificates one for admin(molly) and one for user(jhon)

molly ->  type: admin
jhon -> type: user

-> lets say molly is assigned with a admin certificate, in molly's certificate it will indicate the principle as a acmeMSP.admin
-> lets say jhon is assigned with a user certificate, in jhon's certificate it will indicate the principle as a acmeMSP.member


# policy hieracrchies

- policies ar defined in appropriate section of configtx.yaml file

configtx.yaml

   Organizations: organization level policies
   Orderer: orderer level policies
   Channel: channel level policies


- Readers policy can govern which principle can read
- Writers Policy can govern which principle can write
- Admin Policy can govern which principle can carry out admin actions


examples of actions

- Readers -> reades the channel -> example -> query a chaincode
- Writers -> writes to channel -> example -> invoke a chaincode
- Admins -> admin action on org -> example -> add peer, update anchor peer tx




policy definition has two parts

   Type: Signature Policy OR implicity Meta
   Rule: Boolean expressions


Signature Policy 
-> rules are boolean expressions
-> applicable at all level
-> functions: OR(...), AND(...), OUTOF(...)

Implicit Meta Policy
-> rules refer to other policies
-> aggreated from referred policies
-> applicable for channel configuration & application configuration
-> not applicable for org level policies
-> Keywords like ANY, ALL, MAJORITY


#### if you convert any block file to json file you will see data like below

this is just dummy data dont take it for real
{
   data: 
   data: [
      {
         ......
         channel_group: {
            groups: {
               mod_policy: "Admins" # 1
               policies: ["Admins", "Readers", "Writers"] # 2
               values: { # 3
                  BlockDataHashingStructure
                  HashingAlgorithm
                  OrdererAddresss
               }
            }
         }
      }
   ]
}


-> 1. mod_policy -> who can modify the below policies
-> 2. policies 
-> 3. values -> what they can modify



examples of signature policies


OR('Org1MSP.member') -> any member of org1
OR('Org1MSP.admin', 'Org2MSP.member') -> an admin of org1 or an member of org2

AND('Org1MSP.admin', 'Org2MSP.admin')



```yaml
Organizations:
    - &Org1
        # DefaultOrg defines the organization which is used in the sampleconfig
        # of the fabric.git development environment
        Name: Org1MSP

        # ID to load the MSP definition as
        ID: Org1MSP

        MSPDir: ../organizations/peerOrganizations/org1.example.com/msp

        # Policies defines the set of policies at this level of the config tree
        # For organization policies, their canonical path is usually
        #   /Channel/<Application|Orderer>/<OrgName>/<PolicyName>
        Policies:
            Readers:
                Type: Signature
                Rule: "OR('Org1MSP.admin', 'Org1MSP.peer', 'Org1MSP.client')"
            Writers:
                Type: Signature
                Rule: "OR('Org1MSP.admin', 'Org1MSP.client')"
            Admins:
                Type: Signature
                Rule: "OR('Org1MSP.admin')"
            Endorsement:
                Type: Signature
                Rule: "OR('Org1MSP.peer')"
```

in the above yaml file we have Policies which are explained here

-> Readers -> "OR('Org1MSP.admin', 'Org1MSP.peer', 'Org1MSP.client')" can read(world state)
-> Writers -> "OR('Org1MSP.admin', 'Org1MSP.client')" can write(submit transanction)

-> Admins -> "OR('Org1MSP.admin')" can add peer or update anchor peer tx 

-> Endorsement -> "OR('Org1MSP.peer')" only peer can endorse the transaction




##### you can view organization policies in JSON file in the below location


channel_group
         groups
            Application
               groups
                  acme(organization name)







               

