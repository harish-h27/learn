## fabric-ca-server


### hlf = hyperledger fabric


in hlf identity creation process has two steps\
    a) registering user or peer or orderer, this step is performed by registrer which leaders creation of enrollmentId and secret\
    b) enrolling , this step is done by actual user or admin of peer or orderer with the help of enrollmentId and secret from previous step\


Registrar -> step-1 -> registration process -> generates -> enrollmentId and enrollment secret\
End User -> setp-2 -> enrollment process -> generates -> certificate and private keyt\



### starting a fabric-ca server as docker container
note: for each organization the ca-server will be different\
```yaml
version: '2.1'

networks:
  test:
    name: fabric_test

services:

  ca_org1:
    image: hyperledger/fabric-ca:latest
    environment:
      - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
      - FABRIC_CA_SERVER_CA_NAME=ca-org1
      - FABRIC_CA_SERVER_TLS_ENABLED=true
      - FABRIC_CA_SERVER_PORT=7054
      - FABRIC_CA_SERVER_OPERATIONS_LISTENADDRESS=0.0.0.0:17054
    ports:
      - "7054:7054"
      - "17054:17054"
    command: sh -c 'fabric-ca-server start -b admin:adminpw -d'
    volumes:
      - ../organizations/fabric-ca/org1:/etc/hyperledger/fabric-ca-server
    container_name: ca_org1
    networks:
      - test

```

bootstrap ca-server in background with ca admin username and password\
```sh
    > fabric-ca-server start  -b admin:adminpw -d
```
the above command just registerd the admin still we need to enroll this admin using fabric-ca-client tool\

when fabric-ca-server starts it will look for fabric-ca-server-config.yaml file\
this file conatins every info and where all the configuration certificates and private that\
belongs to this ca will we stored you can provide path to this file in the following way\ 

1st way -> -h flag while running start command or init command (if not provided uses below)\
2nd way -> export FABRIC_CA_SERVER_HOME = <path> (if not provided uses below)\
3rd way -> export FABRIC_CA_HOME = <path> (if not provided uses below)\
4th way -> export CA_CFG_PATH = <path> (if not provided uses below)\
5th way -> current folder\

you can provide every configuration to fabric-ca-server using env variables as well\



## fabric-ca-client

fabric-ca-client is a cli tool to communicate with fabric-ca-server\


```sh
    > fabric-ca-client <command>
    list of commands
    getcainfo -> gets the ca information
    identity -> manage the identities
    register -> register an identity
    enroll -> enroll an identity
    renroll -> re-enroll an identity
    affiliation -> manage affiliations
    revoke -> revoke identity
    gencrl -> generate certificate revocation list
    gencsr -> generate certificate sigining request
    certificate -> manage certificate
```


### enroll 

```sh 
    > fabric-ca-client enroll -u https://admin:adminpw@localhost:7054 --caname ca-org1 --tls.certfiles ${PWD}/organizations/fabric-ca/org1/tls-cert.pem 
```
the above tls-cert.pem file belongs to certificate authority or ca


### list the identities
```sh
fabric-ca-client identity list 
```

### register 
```sh
fabric-ca-client register --flags

falgs with values
--id.name user
--id.secret userpw
--id.affliation org2
--id.maxenrollments 2

ex:
  fabric-ca-client register --caname ca-org1 --id.name peer0 --id.secret peer0pw --id.type peer --tls.certfiles ${PWD}/organizations/fabric-ca/org1/tls-cert.pem
```

the above tls-cert.pem file belongs to certificate authority or ca\


when we enroll to ca we need to give msp path in -M flag where it will create different folders and files in that path or\
we can also specify the path using env variable FABRIC_CA_CLIENT_HOME=<path to store msp >\


## folder structure of fabric-ca-server 

📦org1\
 ┣ 📂msp\
 ┃ ┣ 📂cacerts\
 ┃ ┣ 📂keystore\
 ┃ ┃ ┣ 📜IssuerRevocationPrivateKey\
 ┃ ┃ ┣ 📜IssuerSecretKey\
 ┃ ┃ ┣ 📜db1fb44aae6d091e9fb0101df9e592b27ccf8e0355a288b18fa27d4bca0937ef_sk\
 ┃ ┃ ┗ 📜e5debf93872a888cbcb5796457fdaabc338c54299c5ad16facb7095fb18274fa_sk\
 ┃ ┣ 📂signcerts\
 ┃ ┗ 📂user\
 ┣ 📜IssuerPublicKey\
 ┣ 📜IssuerRevocationPublicKey\
 ┣ 📜ca-cert.pem\
 ┣ 📜fabric-ca-server-config.yaml\
 ┣ 📜fabric-ca-server.db\
 ┗ 📜tls-cert.pem\

msp folder contains\
cacerts (empty)\
keystore (private key of the ca-server itself)\
signcerts (empty)\
user (empty)\

fabric-ca-server-config.yaml (contains configurations used to create this ca-server)\
fabric-ca-server.db (database file where all the info of users, peers and orderers who are members of this ca are stored)\
ca-cert.pem (this the certificate of the ca-server also called as root ca cert)\
tls-cert.pem (this the tls certificate of the ca-server also called as root tls cert)\

__the certificate tls-cert.pem this file will be used every time when we try to communicate with the ca-server in --tls.certfiles flag__ \

example1(enrolling ca admin):\
    \
    ```sh
    export FABRIC_CA_CLIENT_HOME=${PWD}/organizations/peerOrganizations/org1.example.com/
    fabric-ca-client enroll -u https://admin:adminpw@localhost:7054 --caname ca-org1 --tls.certfiles ${PWD}/organizations/fabric-ca/org1/tls-cert.pem
    ```
    \
example2(registering peer):\
    \
    ```sh
    fabric-ca-client register --caname ca-org1 --id.name peer0 --id.secret peer0pw --id.type peer --tls.certfiles ${PWD}/organizations/fabric-ca/org1/tls-cert.pem
    ```\
example3(enrolling for tls certs):\
    \
    ```sh
    fabric-ca-client enroll -u https://peer0:peer0pw@localhost:7054 --caname ca-org1 -M ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp --csr.hosts peer0.org1.example.com --tls.certfiles ${PWD}/organizations/fabric-ca/org1/tls-cert.pem
    ```


while enrolling for tls-cert we will pass extra flag called --csr.hosts\
--csr.hosts peer0.org1.example.com\


### file structure after enrolling org(admin, users, peers, orderers)

the msp you find under the org1.example.com belongs the admin of the ca, other folders are inside org1.example.com are manually created\
📦org1.example.com\
 ┣ 📂ca\
 ┃ ┗ 📜ca.org1.example.com-cert.pem\
 ┣ 📂msp\
 ┃ ┣ 📂cacerts\
 ┃ ┃ ┗ 📜localhost-7054-ca-org1.pem\
 ┃ ┣ 📂keystore\
 ┃ ┃ ┗ 📜02a5aa64c926ae0a53f7134c8646a9977a788d9f4bb151dbd5dc8dcf36fcae77_sk\
 ┃ ┣ 📂signcerts\
 ┃ ┃ ┗ 📜cert.pem\
 ┃ ┣ 📂tlscacerts\
 ┃ ┃ ┗ 📜ca.crt\
 ┃ ┣ 📂user\
 ┃ ┣ 📜IssuerPublicKey\
 ┃ ┣ 📜IssuerRevocationPublicKey\
 ┃ ┗ 📜config.yaml\
 ┣ 📂peers\
 ┃ ┗ 📂peer0.org1.example.com\
 ┃ ┃ ┣ 📂msp\
 ┃ ┃ ┃ ┣ 📂cacerts\
 ┃ ┃ ┃ ┃ ┗ 📜localhost-7054-ca-org1.pem\
 ┃ ┃ ┃ ┣ 📂keystore\
 ┃ ┃ ┃ ┃ ┗ 📜126e1a7f720553e86c1d2b981eabcbd1607bd6e053df6f896785f610e98f1056_sk\
 ┃ ┃ ┃ ┣ 📂signcerts\
 ┃ ┃ ┃ ┃ ┗ 📜cert.pem\
 ┃ ┃ ┃ ┣ 📂user\
 ┃ ┃ ┃ ┣ 📜IssuerPublicKey\
 ┃ ┃ ┃ ┣ 📜IssuerRevocationPublicKey\
 ┃ ┃ ┃ ┗ 📜config.yaml\
 ┃ ┃ ┗ 📂tls\
 ┃ ┃ ┃ ┣ 📂cacerts\
 ┃ ┃ ┃ ┣ 📂keystore\
 ┃ ┃ ┃ ┃ ┗ 📜02c9c4ddb46a8e8649392dcc7277b5e8b43630bda4ea9221cdbe307c28ae5f47_sk\
 ┃ ┃ ┃ ┣ 📂signcerts\
 ┃ ┃ ┃ ┃ ┗ 📜cert.pem\
 ┃ ┃ ┃ ┣ 📂tlscacerts\
 ┃ ┃ ┃ ┃ ┗ 📜tls-localhost-7054-ca-org1.pem\
 ┃ ┃ ┃ ┣ 📂user\
 ┃ ┃ ┃ ┣ 📜IssuerPublicKey\
 ┃ ┃ ┃ ┣ 📜IssuerRevocationPublicKey\
 ┃ ┃ ┃ ┣ 📜ca.crt\
 ┃ ┃ ┃ ┣ 📜server.crt\
 ┃ ┃ ┃ ┗ 📜server.key\
 ┣ 📂tlsca\
 ┃ ┗ 📜tlsca.org1.example.com-cert.pem\
 ┣ 📂users\
 ┃ ┣ 📂Admin@org1.example.com\
 ┃ ┃ ┗ 📂msp\
 ┃ ┃ ┃ ┣ 📂cacerts\
 ┃ ┃ ┃ ┃ ┗ 📜localhost-7054-ca-org1.pem\
 ┃ ┃ ┃ ┣ 📂keystore\
 ┃ ┃ ┃ ┃ ┗ 📜bdd3abb11fe5d430761d227000537b626f39e97c0f711765b2cefc2a2205001e_sk\
 ┃ ┃ ┃ ┣ 📂signcerts\
 ┃ ┃ ┃ ┃ ┗ 📜cert.pem\
 ┃ ┃ ┃ ┣ 📂user\
 ┃ ┃ ┃ ┣ 📜IssuerPublicKey\
 ┃ ┃ ┃ ┣ 📜IssuerRevocationPublicKey\
 ┃ ┃ ┃ ┗ 📜config.yaml\
 ┃ ┗ 📂User1@org1.example.com\
 ┃ ┃ ┗ 📂msp\
 ┃ ┃ ┃ ┣ 📂cacerts\
 ┃ ┃ ┃ ┃ ┗ 📜localhost-7054-ca-org1.pem\
 ┃ ┃ ┃ ┣ 📂keystore\
 ┃ ┃ ┃ ┃ ┗ 📜1b127e2eb7c3bb26fef125db86ce7280128da2011f38dab96e0be2620bf706a6_sk\
 ┃ ┃ ┃ ┣ 📂signcerts\
 ┃ ┃ ┃ ┃ ┗ 📜cert.pem\
 ┃ ┃ ┃ ┣ 📂user\
 ┃ ┃ ┃ ┣ 📜IssuerPublicKey\
 ┃ ┃ ┃ ┣ 📜IssuerRevocationPublicKey\
 ┃ ┃ ┃ ┗ 📜config.yaml\
 ┣ 📜connection-org1.json\
 ┣ 📜connection-org1.yaml\
 ┗ 📜fabric-ca-client-config.yaml\


 lets explore one by one\

 under org1.example.com we have this folder\

 ca(we copied same ca root certificate and changed the name)\
 msp(contains msp config about ca admin)\
 peers(contains msp about all the peers of this org)\
 tlsca(we copied same ca root certificate and changed the name)\
 users(contains msp of all the users of this org)\


### msp folder (this folder structure is same for all admins and users who are present in users folder)

cacerts(we copied same ca root certificate and changed the name)\
keystore(contains private key that belongs to this admin)\
signcerts(contains public certificate that belongs to this admin)\
tlscacerts(we copied same ca root certificate and changed the name)\
user(empty)\
config.yaml (we need to create it manually, this file is same for all the components in this org)\


## peers folder
peers folder contains separte folder for each peer inside of\

### peer0.org1.example.com
under this folder we have \
msp folder\
tls folder\

### msp folder of peer 

cacerts(we copied same ca root certificate and changed the name)\
keystore(contains private key that belongs to this peer)\
signcerts(contains public certificate that belongs to this peer)\
user(empty)\
config.yaml\


### tls folder of peer


cacerts(empty)\
keystore(contains tls private key that belongs to this admin)\
signcerts(contains public tls-certificate that belongs to this peer)\
tlscacerts(we copied same ca root certificate and changed the name)\
user(empty)\
ca.crt(we copied same ca root certificate and changed the name, we just named it ca.crt because of simplicity)\
server.crt(public tls-certificate we coppied it from above signcerts folder and renamed it server.crt for simplicity)\
server.key(tls private key we coppied it from above keystore folder and renamed it server.key for simplicity)\

