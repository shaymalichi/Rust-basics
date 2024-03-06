 # Set Up an Ethereum Proof-of-Stake Devnet 
Today, running an Ethereum node require  **two components**:
1.  **execution client software**  in charge of processing transactions and smart contracts. Example of execution client softwares are:  [go-ethereum](https://geth.ethereum.org/),  [besu](https://besu.hyperledger.org/),  [erigon](https://github.com/ledgerwatch/erigon),  [nethermind](https://nethermind.io/)  or  [reth](https://paradigmxyz.github.io/reth/).
2.  **consensus client software**  in charge of running the proof-of-stake logic. This tutorial will use the  [Prysm](https://github.com/prysmaticlabs/prysm)  implementation, which my team develops.

## Easy setup using Docker

To get started, install  [Docker](https://docs.docker.com/get-docker/)  for your system.

Next, clone a repository containing the configuration needed to run a local devnet with Docker here:

```
git clone https://github.com/Offchainlabs/eth-pos-devnet
```
This repository provides a docker-compose file to run a fully-functional, local development network for Ethereum with proof-of-stake enabled. This configuration uses [Prysm](https://github.com/prysmaticlabs/prysm) as a consensus client and [go-ethereum](https://github.com/ethereum/go-ethereum) for execution.
This sets up a single node development network with 64 deterministically-generated validator keys to drive the creation of blocks in an Ethereum proof-of-stake chain. check the repository for more info. 

* The genesis.json file can be adjusted as needed to add accounts before setting up the containers. 

Finally, simply run docker compose inside of the repository above.
```
docker compose up -d
```

3 active containers should be active while the network is running, 
the execution node, and validators node, the consensus algorithm.

-   geth JSON-RPC API is available at  http://localhost:8545
- prysm client's REST APIs are available at http://localhost:3500
- prysm client also exposes a gRPC API at http://beacon-chain:4000

execution node public address is: 
```
0x123463a4b065722e99115d6c222f267d9cabb524
```
the private key is: 
```
2e0834786285daccd064ca17f1654f67b4aef298acbb82cef9ec422fb4975622
```

## interacting with the network
Connect to the go ethereum console with
```
docker exec -it <container_id> geth attach ipc:/execution/geth.ipc
```
Check the balance of your  `0x123463a4b065722e99115d6c222f267d9cabb524`  address:

```
web3.fromWei(eth.getBalance("0x123463a4B065722E99115D6c222f267d9cABb524"), "ether")
```
It should output  `20000`. Those  `20.000 ETH`  correspond to what have been set in the  `genesis.json`  file of the address  `0x123463a4b065722e99115d6c222f267d9cabb524`.

Because we previously imported into go-ethereum the private key corresponding to the address  `0x123463a4b065722e99115d6c222f267d9cabb524`, we have the control of this account.

Using the go-ethereum console, we will send some Ethers from  `0x123463a4b065722e99115d6c222f267d9cabb524`  to  `0x123c0ffee567beef890decade123fade456bed78`.

Check the balance of  `0x123c0ffee567beef890decade123fade456bed78`:

```
web3.fromWei(eth.getBalance("0x123c0ffee567beef890decade123fade456bed78"), "ether")
```

This account was not part of the  `genesis.json`  and nobody sent any ETH to it, so it should output  `0`.

Now, send  `1 ETH`  from our address  `0x123463a4b065722e99115d6c222f267d9cabb524`  to the address  `0x123c0ffee567beef890decade123fade456bed78`.

```
web3.eth.sendTransaction({    from: "0x123463a4b065722e99115d6c222f267d9cabb524",    to: "0x123c0ffee567beef890decade123fade456bed78",    value: web3.toWei(1, "ether")})
```
