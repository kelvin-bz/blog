---
title: Blockchain Basics P4 - Truffle Pet-Shop Smart Contract 
categories: [blockchain, solidity]
date: 2024-07-09 00:00:00
tags: [solidity, smart-contracts]
image: "/assets/images/pet-shop-smart-contract.jpg"
published: false
---

Truffle is the development framework for Ethereum, simplifying smart contract creation, testing, and deployment. 

Read more about Truffle [here](https://archive.trufflesuite.com/guides/pet-shop/).

## About the Pet Shop Tutorial

The Truffle Pet Shop tutorial is a great place to start learning about Ethereum development. It's a simple dApp that allows users to adopt pets. 

## Setting Up Truffle

Install Truffle globally using npm:
```bash
npm install -g truffle
```

To kickstart your project, begin by unboxing the "pet-shop" example:

```bash
truffle unbox pet-shop
```



## Directory Structure

The directory structure of a Truffle project is similar to the following:


```mermaid
graph TD
    root["fa:fa-folder Project Root"]

    subgraph truffleFolders["<i class='fas fa-folder-open'></i> Truffle Folders"]
        style truffleFolders fill:#9f9,stroke:#333,stroke-width:2px
        contracts["fa:fa-folder-open contracts/"]
        migrations["fa:fa-folder-open migrations/"]
        test["fa:fa-folder-open test/"]
    end

    root --> contracts
    root --> migrations
    root --> test
    root --> truffleConfig["fa:fa-cog truffle-config.js"]

    subgraph contractsFiles["<i class='fas fa-file-code'></i> Contract Files"]
        style contractsFiles fill:#f9f,stroke:#333,stroke-width:2px
        adoptionSol["Adoption.sol"]
        migrationsSol["Migrations.sol"]
    end

    contracts --> contractsFiles

    subgraph testFiles["<i class='fas fa-file-code'></i> Test Files"]
        style testFiles fill:#ff9,stroke:#333,stroke-width:2px
        testAdoptionSol["TestAdoption.sol (Solidity)"]
        testAdoptionJs["testAdoption.js (JavaScript)"]
    end

    test --> testFiles
```

## Create a Contract

```js
// contracts/Adoption.sol
pragma solidity ^0.5.0;

contract Adoption {
    // This specifies an array of 16 elements where each element is of type address.
    address[16] public adopters;
    // Adopting a pet
    function adopt(uint petId) public returns (uint) {
        require(petId >= 0 && petId <= 15);
        adopters[petId] = msg.sender;
        return petId;
    }

    function getAdopters() public view returns (address[16] memory) {
        return adopters;
    }
}
```


This specifies an array of 16 elements where each element is of type address. The address type in Solidity is used to store Ethereum addresses, which are 20-byte values that uniquely identify accounts (both user accounts and contract accounts) on the Ethereum blockchain. `adopters` would typically be used to map pet IDs to the addresses of users who have adopted them. 

**Example**:

Make a call to the `adopt` function with the argument `8` to adopt the pet with ID 8. The function will store the address of the account that made the call (msg.sender) in the `adopters` array at index 8.


```mermaid
graph LR

    subgraph Caller["<i class='fas fa-user'></i> Caller (msg.sender)"]
        style Caller fill:#add8e6,stroke:#333,stroke-width:2px
        callerAddress["0x59...eDe2 (Caller's Address)"]
    end

    Caller --> |"adopt(8)"|adopters8a
    
    subgraph afterAdopt["adopeters"]
        adopters0a["0x00... (Empty)"]
        adopters1a["0x00... (Empty)"]
        adopters2a[...]
        adopters8a["0x59...eDe2 (Adopter at 8)"]:::highlight
        adopters9a[...]
        adopters16a["0x00... (Empty)"]
    end

    classDef highlight fill:#ffff99,stroke:#333,stroke-width:2px;
```

## Compile the Contract
```mermaid
graph TD
    SolidityContracts["<i class='fas fa-file-code'></i> .sol Files<br/>(Solidity Smart Contracts)"]
    style SolidityContracts fill:#9f9,stroke:#333,stroke-width:2px;

    TruffleCompile["<i class='fas fa-hammer'></i> Truffle Compile<br/>(solc Compiler)"]

    SolidityContracts --> TruffleCompile --> BuildArtifacts["<i class='fas fa-folder-open'></i> build/contracts/<br/>(Contract Artifacts)"]
    style BuildArtifacts fill:#f9f,stroke:#333,stroke-width:2px;

    subgraph BuildArtifacts
        bytecode["<i class='fas fa-microchip'></i> Bytecode<br/>(EVM Instructions)"]
        style bytecode fill:#ff9,stroke:#333,stroke-width:2px;
        abi["<i class='fas fa-file-signature'></i> ABI<br/>(Contract Interface)"]
        style abi fill:#99f,stroke:#333,stroke-width:2px;
    end
```

When you run the truffle compile command, Truffle utilizes the Solidity compiler (`solc`) behind the scenes.
The compiler translates your human-readable Solidity code into a low-level format called bytecode that can be understood and executed by the Ethereum Virtual Machine (EVM).


To compile the contract, run the following command:

```bash
truffle compile
```

After running the compile command, you should see a new build/ directory in your project. This directory contains all the files generated by the compilation process.

**build/contracts/ (Contract Artifacts)**

The compilation process generates JSON files representing your compiled contracts. These files, called "artifacts," are stored in the build/contracts directory of your Truffle project.
Each artifact file contains two crucial components:
- `Bytecode`: This is the binary representation of your contract's code, the set of instructions that the EVM will run on the blockchain.
- `ABI (Application Binary Interface)`: This is a JSON object that describes the contract's interface, including its functions, their input parameters, return types, and events. The ABI is essential for interacting with your deployed contract from external applications and tools.


```bash
pet-shop/
├── contracts/
│   └── Adoption.sol
└── build/
    └── contracts/
        ├── Adoption.json   // Contains bytecode and ABI
```

Output similar to the following:

```bash
Compiling your contracts...
===========================
> Compiling ./contracts/Adaption.sol
> Compiling ./contracts/Migrations.sol
> Artifacts written to /Users/kelvinbz/source-code/introduction-web3js/c4-pet-shop/build/contracts
> Compiled successfully using:
- solc: 0.5.16+commit.9c3226ce.Emscripten.clang
```

## Ganache: A Local Blockchain

**Ganache** is a personal blockchain for Ethereum development you can use to deploy contracts, develop your applications, and run tests.  It acts as a simulated Ethereum network on your local machine, allowing you to test your smart contracts and decentralized applications (dApps) without incurring real costs or risking funds on a live network. 

![ganache]({{ site.baseurl }}/assets/images/ganache.png)

For more information, visit the [Ganache website](https://archive.trufflesuite.com/ganache/).

After installing Ganache, start the application and quickly create a new workspace by clicking the "Quickstart" button. This will create a new workspace with 10 accounts, each loaded with 100 fake Ether (ETH).

### Configure Truffle to Connect to Ganache

To connect Truffle to Ganache, you need to update the truffle-config.js file in your project directory. This file contains the configuration settings for your Truffle project, including the network settings for connecting to Ethereum networks.

```javascript
// truffle-config.js
module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // for more about customizing your Truffle configuration!
  networks: {
    development: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "*" // Match any network id
    },
    develop: {
      port: 8545
    }
  }
};
```

## Run Migrations

```mermaid
graph 
    subgraph migrations["<i class='fas fa-shoe-prints'></i> Migrations"]
        style migrations fill:#f0f8ff,stroke:#000,stroke-width:2px;
        migrationScripts["<i class='fas fa-file-alt'></i> Migration Scripts (.js)"]
    end
    
    subgraph artifacts["<i class='fas fa-archive'></i> Contract Artifacts"]
        style artifacts fill:#e0ffff,stroke:#000,stroke-width:2px;
        bytecodeAbi["<i class='fas fa-file-code'></i> Bytecode & ABI (.json)"]
    end

    subgraph deployment["<i class='fas fa-rocket'></i> Deployment"]
        style deployment fill:#d8bfd8,stroke:#000,stroke-width:2px;
        truffleMigrate["<i class='fas fa-running'></i> truffle migrate"]
        network["<i class='fas fa-network-wired'></i> Blockchain Network"]
    end
    
    subgraph migrationsContract["<i class='fas fa-file-contract'></i> Migrations Contract"]
        style migrationsContract fill:#f5f5dc,stroke:#000,stroke-width:2px;
    end
    
    subgraph blockchain["<i class='fas fa-cubes'></i> Blockchain"]
        style blockchain fill:#fff0f5,stroke:#000,stroke-width:2px;
        deployedContracts["<i class='fas fa-check-circle'></i> Deployed Contracts"]
    end


    migrations -->|"Reads ABI from & Deploys Bytecode from"| artifacts
    artifacts & migrations -->|"Executes & Tracks"| deployment
    deployment -->|"Updates"| migrationsContract
    deployment -->|"Sends Transactions to"| blockchain
    migrationsContract -->|"Records Deployment History on"| blockchain
```

You start by writing migration scripts (JavaScript files) in the migrations/ directory. These scripts define how your contracts should be deployed to the blockchain. Each migration script typically deploys a single contract or performs a specific task.

```javascript
// migrations/2_deploy_contracts.js
var Adoption = artifacts.require("Adoption");
module.exports = function(deployer) {
    deployer.deploy(Adoption);
};
```

Run the following command to migrate your contract to the blockchain:

```bash
truffle migrate
```

you should see output similar to the following:

```bash
Starting migrations...
======================
> Network name:    'development'
> Network id:      5777
> Block gas limit: 6721975 (0x6691b7)


1_initial_migration.js
======================

   Deploying 'Migrations'
   ----------------------
   > transaction hash:    0x9f5053929624c42c11b9216359f0c9fe3d911293f37b71c54fb0c85e5510237a
   > Blocks: 0            Seconds: 0
   > contract address:    0x81634f10231fB77FB5d5f4ECBb6095C348491f40
   > block number:        1
   > block timestamp:     1720926701
   > account:             0x594eAe4dB0BD6AD48C42864f0492CA735017eDe2
   > balance:             99.999347804875
   > gas used:            193243 (0x2f2db)
   > gas price:           3.375 gwei
   > value sent:          0 ETH
   > total cost:          0.000652195125 ETH

   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:      0.000652195125 ETH

Summary
=======
> Total deployments:   1
> Final cost:          0.000652195125 ETH


(base) @Kenvinbz c4-pet-shop % truffle migrate

Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.


Starting migrations...
======================
> Network name:    'development'
> Network id:      5777
> Block gas limit: 6721975 (0x6691b7)


2_deploy_contracts.js
=====================

   Deploying 'Adoption'
   --------------------
   > transaction hash:    0xd4a7d87e2b5cc7ccfdb011aabee6c8d2b60271bff16916616d1c8a35f1792f10
   > Blocks: 0            Seconds: 0
   > contract address:    0x8cF8593297FB5EA2F01b2aDF8fc29745896955AD
   > block number:        3
   > block timestamp:     1720927011
   > account:             0x594eAe4dB0BD6AD48C42864f0492CA735017eDe2
   > balance:             99.998550649218314381
   > gas used:            203827 (0x31c33)
   > gas price:           3.176737487 gwei
   > value sent:          0 ETH
   > total cost:          0.000647504871762749 ETH

   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:     0.000647504871762749 ETH

Summary
=======
> Total deployments:   1
> Final cost:          0.000647504871762749 ETH

```

- **Initial Migration (1_initial_migration.js)**:

This script deploys the `Migrations` contract to your local blockchain (Development network with ID 5777). The Migrations contract is a special contract that helps Truffle track which migrations have already been run.
It cost 0.000652195125 ETH (fake Ether since it's a development network) to deploy, including gas fees. (check this post [Gas Price and Gas Limit](https://kelvin-bz.github.io/posts/blockchain-for-dummies/#gas-fees))

- **Deploying Contracts (2_deploy_contracts.js)**:

This script deploys your custom `Adoption` contract.
It also cost 0.000647504871762749 ETH on the development network.


- **Contract Address**: The contract address is a unique identifier assigned to a smart contract when it is deployed on the Ethereum blockchain. It allows users and other contracts to interact with the deployed contract.
  - `Migrations Contract` was deployed at 0x81634f10231fB77FB5d5f4ECBb6095C348491f40.
  - `Adoption Contract` was deployed at 0x8cF8593297FB5EA2F01b2aDF8fc29745896955AD.
  
Each time you run truffle migrate, a new contract address will be generated ONLY if you're deploying a NEW contract or a new version of an existing contract.

If you're simply re-running a migration script for an already deployed contract, Truffle will recognize that the contract exists on the blockchain and won't redeploy it, thus the contract address remains the same.



```mermaid
graph TD
    subgraph migrations["<i class='fas fa-shoe-prints'></i> Migrations"]
        style migrations fill:#f0f8ff,stroke:#000,stroke-width:2px;
        migration1["1_initial_migration.js"]
        migration2["2_deploy_contracts.js"]
    end

    subgraph contracts["<i class='fas fa-file-code'></i> Contracts"]
        style contracts fill:#e0ffff,stroke:#000,stroke-width:2px;
        migrationsContract["Migrations.sol"]
        adoptionContract["Adoption.sol"]
    end

subgraph blockchain["<i class='fas fa-cubes'></i> Blockchain (Development)"]
style blockchain fill:#d8bfd8,stroke:#000,stroke-width:2px;
deployedMigrations["0x... (Migrations)"]
deployedAdoption["
            0x... (Adoption)"]
end

contracts -->|Deployed on| blockchain
migration1 -->|"Depends on"| migration2
migrations --deploy--> contracts
```


## Test the Contract

Add a new file in the test/ directory called testAdoption.js. This file will contain the test suite for the Adoption contract.

```js
//test/testAdoption.test.js
const Adoption = artifacts.require("Adoption");

contract("Adoption", (accounts) => {
    let adoption;
    let expectedAdopter;

    before(async () => {
        adoption = await Adoption.deployed();
    });

    describe("adopting a pet and retrieving account addresses", async () => {
        before("adopt a pet using accounts[0]", async () => {
            await adoption.adopt(8, { from: accounts[0] });
            expectedAdopter = accounts[0];
        });

        it("can fetch the address of an owner by pet id", async () => {
            const adopter = await adoption.adopters(8);
            assert.equal(adopter, expectedAdopter, "The owner of the adopted pet should be the first account.");
        });

        it("can fetch the collection of all pet owners' addresses", async () => {
            const adopters = await adoption.getAdopters();
            assert.equal(adopters[8], expectedAdopter, "The owner of the adopted pet should be in the collection.");
        });
    });
});

```

- `{ from: accounts[0] }` part specifies the sender of the transaction
- When you call await adoption.adopt(8, { from: accounts[0] });, you are sending a transaction from accounts[0] to the adopt function of the Adoption contract


Run the test suite using the following command:

```bash
truffle test
```

you should see output similar to the following:

```bash
Using network 'development'.


Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.


  Contract: Adoption
    adopting a pet and retrieving account addresses
      ✔ can fetch the address of an owner by pet id
      ✔ can fetch the collection of all pet owners' addresses


  2 passing (92ms)
```

## Source Code

Full source code for the Truffle Pet Shop tutorial can be found on GitHub here: [Truffle Pet Shop](https://github.com/kelvin-bz/smart-contract-pet-shop)


## References

- Read more about Truffle [https://archive.trufflesuite.com/guides/pet-shop/](https://archive.trufflesuite.com/guides/pet-shop/).


<a href="/posts/truffle-pet-shop-interface">Next Post: Blockchain Basics P5 - Truffle Pet-Shop Interface</a> 
