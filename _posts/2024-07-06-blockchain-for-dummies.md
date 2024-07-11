---
title: Blockchain for Dummies
categories: [tech]
date: 2024-07-06 08:00:00
image: "/assets/images/blockchain-for-dummies.png"
tags: [blockchain]
---

I am not a blockchain expert, but I have been learning about blockchain technology and its applications. I have created some diagrams to help me understand the concepts better. I hope these diagrams can help you too.



## Transaction - Block

```mermaid
graph LR
    subgraph TransactionData["Transaction Data"]
        style TransactionData fill:#FFF2CC,stroke:#D6B656
        TransactionDetails["Transaction Details<br/>(Sender, Recipient, Amount)"]
    end

    subgraph Keys["Keys"]
        style Keys fill:#C2E0C6,stroke:#5FB483
        PrivateKey["Private Key (Sender)"]
        PublicKey["Public Key (Sender/Recipient)"]
    end
    PrivateKey --"Signs"--> TransactionSignature
    PublicKey --"Verifies"--> TransactionSignature
    TransactionDetails --"Included in"--> Transaction

    subgraph Transaction["Transaction"]
      style Transaction fill:#E1D5E7,stroke:#9673A6
      TransactionSignature["Digital Signature"]
    end

    subgraph Verification["Verification"]
      style Verification fill:#D4EDDA,stroke:#4CAF50
      BlockchainNetwork["Blockchain Network"]
    end

    Transaction --"Verified by"--> BlockchainNetwork


    subgraph Block["Block (Example)"]
        style Block fill:#F0F0F0,stroke:#888888
        BlockHeader["Block Header"]
        BlockData["Block Data (Multiple Transactions)"]
    end

    Transaction -.-> BlockData
    BlockHeader --> BlockHash["Block Hash"]
    BlockHeader --> PreviousBlockHash["Previous Block's Hash"]
    BlockHeader --> Nonce["Nonce"]
```

- **Transaction**: A digital record of value transfer between two parties (sender and recipient) with a specific amount. It's signed with the sender's private key and verified by the network using their public key. [ethereum's doc](https://ethereum.org/en/developers/docs/transactions/)


- **Block**: A collection of verified transactions grouped together, along with a header containing metadata (e.g., timestamp, previous block's hash).


## Miner - Node - Wallet

```mermaid
graph LR
    subgraph Network["Blockchain Network"]
        style Network fill:#F2F2F2,stroke:#666666
        Transactions["Transactions"]
        style Transactions fill:#E1D5E7,stroke:#9673A6
        Blocks["Blocks"]
        style Blocks fill:#F0F0F0,stroke:#888888
    end

    subgraph Participants["Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Wallet["Wallet (User)"]
        style Wallet fill:#FFF2CC,stroke:#D6B656
        Node["Node"]
        style Node fill:#C2E0C6,stroke:#5FB483
        Miner["Miner"]
        style Miner fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Wallet
      PublicKey["Public Key (Sender/Recipient)"]
      PrivateKey["Private Key (Sender)"]
    end


    Wallet -- "Creates/Receives" --> Transactions
    Node -- "Validates/Stores" --> Transactions
    Node -- "Validates/Stores" --> Blocks
    Miner -- "Creates/Validates" --> Blocks
    Transactions -.-> Blocks

    PrivateKey -- "Signs" --> Transactions
    PublicKey -- "Verifies" --> Transactions
    Miner -- "Receives Rewards From" --> Blocks

```

- **Miner**: Miners solve complex puzzles to add new blocks to the blockchain, ensuring its security and earning rewards in the process.

- **Node**: Nodes maintain copies of the blockchain, validate transactions, and relay information across the network.

- **Wallet**:  Wallets store users' private keys (used for signing transactions) and public keys (used for receiving transactions), allowing them to interact with the blockchain.
##  Immutability and Consensus

```mermaid
graph TD
        block1["Block 1"]
        block2["Block 2"]
        block3["Block 3"]:::invalid
        block4["Block 4"]:::invalid
        block5["Block 5"]:::invalid

        block1 -->|Hash Link| block2
        block2 -->|Hash Link| block3
        block3 -->|Hash Link| block4
        block4 -->|Hash Link| block5
    
    block2 -- "Data Change" --> invalidBlock2["Invalid Block 2"]
    invalidBlock2 -.-> |"Invalidates"| block3
    invalidBlock2 -.-> |"Invalidates"| block4
    invalidBlock2 -.-> |"Invalidates"| block5
    classDef invalid fill:#f96

```

**Immutability**:This means that once a block of data is added to the blockchain, it is extremely difficult to alter or remove. This immutability is crucial for maintaining trust and security in the network. Each block in a blockchain contains a unique cryptographic hash that is calculated based on the block's contents and the hash of the previous block. If you change any data within a block, its hash changes, invalidating all subsequent blocks in the chain.

```mermaid
graph LR
    subgraph Blockchain["Blockchain"]
        style Blockchain fill:#ff9,stroke:#333,stroke-width:2px
        Block1["Block 1"]
        Block2["Block 2"]
        Block3["Block 3"]

        Block1 --> |"Hash Link"| Block2
        Block2 --> |"Hash Link"| Block3
    end

    Block2 -- "Attempted Data Change" -->  Disagreement["Disagreement with\nNetwork Consensus"]

```

**Consensus**: Changes to the blockchain require the agreement (consensus) of the majority of the network's nodes. To modify a historical block, you'd need to convince the majority of nodes to rewrite the entire chain from that point onwards, which is computationally infeasible and extremely unlikely.



## P2P (Peer-to-Peer) Network

A peer-to-peer (P2P) network is a type of network architecture where all participants (devices or computers) have equal status and capabilities.  Unlike traditional client-server models, there's no central authority governing the network. Instead, each participant, known as a node, directly communicates and shares resources with other nodes.

```mermaid
graph TD
    subgraph P2PBlockchainNetwork["Peer-to-Peer Blockchain Network"]
        A[Node 1]
        B[Node 2]
        C[Node 3]
        A <--"Share Ledger Data & Validate Transactions"--> B
        A <--> C
        B <--> C
    end
```


## Hashrate

```mermaid
graph LR
    HashRate["BTC Hash Rate<br/>614.42 EH/s"]
    style HashRate fill:#F2F2F2,stroke:#666666
    HashRate -- "Represents" --> TotalNetworkPower["Total Network Computing Power"]
    style TotalNetworkPower fill:#FFDDCC,stroke:#E69966

    TotalNetworkPower -- "Influenced by" --> NumberOfMiners["Number of Miners"]
    style NumberOfMiners fill:#FFF2CC,stroke:#D6B656

    TotalNetworkPower -- "Influenced by" --> MiningHardware["Mining Hardware Efficiency"]
    style MiningHardware fill:#C2E0C6,stroke:#5FB483

    HashRate -- "Determines" --> Difficulty["Mining Difficulty"]
    style Difficulty fill:#E1D5E7,stroke:#9673A6

    HashRate -- "Affects" --> BlockTime["Average Block Time<br/>~10 minutes"]
    style BlockTime fill:#D5E8D4,stroke:#82B366
    
    HashRate -- "Indicates" --> Security["Network Security"]
    style Security fill:#9673A6,stroke:#E1D5E7
```

**Bitcoin Hashrate** measures the total computing power miners use to secure the network, currently at 614.42 EH/s. A higher hashrate enhances security by making it harder for bad actors to control the network. It also influences mining difficulty, ensuring consistent block creation times.

## ETH (proof of stake)

```mermaid
graph LR
    subgraph Network["Blockchain Network (Ethereum 2.0)"]
        style Network fill:#F2F2F2,stroke:#666666
        Transactions["Transactions"]
        style Transactions fill:#E1D5E7,stroke:#9673A6
        Blocks["Blocks"]
        style Blocks fill:#F0F0F0,stroke:#888888
    end

    subgraph Participants["Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Wallet["Wallet (User)"]
        style Wallet fill:#FFF2CC,stroke:#D6B656
        Node["Node"]
        style Node fill:#C2E0C6,stroke:#5FB483
        Validator["Validator"]
        style Validator fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Wallet
      PublicKey["Public Key (Sender/Recipient)"]
      PrivateKey["Private Key (Sender)"]
    end

    Wallet -- "Creates/Receives" --> Transactions
    Node -- "Validates/Stores" --> Transactions
    Node -- "Validates/Stores" --> Blocks
    Validator -- "Proposes/Validates" --> Blocks
    Transactions -.-> Blocks

    PrivateKey -- "Signs" --> Transactions
    PublicKey -- "Verifies" --> Transactions
    Validator -- "Receives Rewards From" --> Blocks
    Validator --> StakedETH["Staked ETH (32 ETH)"]
    

```

**POS (proof of stake)**: stake ETH to become validators. Validators are randomly selected to propose new blocks, with others attesting to their validity. Correct validators earn rewards, while incorrect ones are penalized. This replaces mining, making Ethereum more scalable and eco-friendly.

## Gas Fees
Gas in the Ethereum network is a unit that measures the computational work required to execute transactions or smart contracts. Each operation on the Ethereum Virtual Machine (EVM) requires a certain amount of gas, which must be paid for in Ether (ETH). Gas ensures that transactions and contract executions are fairly priced according to their resource consumption. Users specify a gas limit and a gas price for their transactions; if the gas limit is exceeded, the transaction fails but the gas used is still deducted. This mechanism prevents network abuse and incentivizes efficient code execution. 

```mermaid
graph TD
    subgraph UserA["Bob's Account"]
        style UserA fill:#9f9,stroke:#333,stroke-width:2px
        BalanceA["1.0042 ETH"]
    end

        style Blockchain fill:#ff9,stroke:#333,stroke-width:2px
        Miner["Miner/Validator"]
    
    subgraph UserB["Alice's Account"]
        style UserB fill:#9f9,stroke:#333,stroke-width:2px
        BalanceB["0 ETH"]
    end

    UserA --"Sends 1 ETH + 0.0042 ETH Fee"--> Blockchain
    Blockchain --"Transfers 1 ETH"--> UserB
    Blockchain --"Pays 0.00021 ETH Tip"--> Miner
    Blockchain --"Burns 0.00399 ETH"--> LostETH["0.00399 ETH"]

```

- Transaction Cost: Sending ETH has a base cost of 21,000 units of gas.
- Gas Price: Bob sets a gas price of 200 gwei (190 gwei base fee + 10 gwei tip).
- Total Fee: The total fee Bob pays is 21,000 gas * 200 gwei/gas = 4,200,000 gwei (or 0.0042 ETH).
- Bob's account is debited 1.0042 ETH (1 ETH for Alice + 0.0042 ETH fee).
- Alice's account is credited 1 ETH.
0.00399 ETH is burned (removed from circulation).
- The miner receives 0.00021 ETH as a tip.

## Stablecoins

```mermaid
graph LR
    classDef cadetblue fill:#5F9EA0,stroke:#333,stroke-width:2px;
    classDef mediumaquamarine fill:#66CDAA,stroke:#333,stroke-width:2px;
    classDef lightgreen fill:#90EE90,stroke:#333,stroke-width:2px;
    classDef lightpink fill:#FFB6C1,stroke:#333,stroke-width:2px;

    subgraph Stablecoins["**Types of Stablecoins**"]
        FiatBacked["Fiat-Backed"]:::cadetblue
        CommodityBacked["Commodity-Backed"]:::mediumaquamarine
        CryptoBacked["Crypto-Backed"]:::lightgreen
        Algorithmic["Algorithmic"]:::lightpink
    end

    FiatBacked --> Tether["Tether (USDT)"]:::cadetblue
    FiatBacked --> USDCoin["USD Coin (USDC)"]:::cadetblue

    CommodityBacked --> TetherGold["Tether Gold (XAUT)"]:::mediumaquamarine
    CommodityBacked --> PAXGold["PAX Gold (PAXG)"]:::mediumaquamarine

    CryptoBacked --> Dai["Dai (DAI)"]:::lightgreen

    Algorithmic --> AMPL["Ampleforth (AMPL)"]:::lightpink
    Algorithmic --> UST["TerraUSD (UST)"]:::lightpink

```

**Stablecoins** offer stability in the volatile crypto market, making them suitable for everyday transactions, remittances, and trading. They provide easy access to the crypto world without exposure to extreme price fluctuation. They are a bridge between traditional finance and the crypto world. 

**Fiat-Backed Stablecoins**: These stablecoins are backed 1:1 by fiat currencies like the US Dollar or Euro. The value of each coin is directly tied to the value of the fiat currency.



**Commodity-Backed Stablecoins**: These stablecoins are backed by physical assets such as precious metals. The value is tied to the commodity, providing stability based on the physical asset's value.


**Crypto-Backed Stablecoins:** These stablecoins are backed by other cryptocurrencies. They are usually over-collateralized to account for the volatility of the underlying crypto assets.


**Algorithmic Stablecoins**: These stablecoins use algorithms and smart contracts to manage the supply and stabilize the value without needing reserves. They rely on mechanisms to adjust the coin supply based on demand.



## Restaking

```mermaid
graph LR
subgraph EthereumBlockchain["Ethereum Blockchain (Beacon Chain)"]
    style EthereumBlockchain fill:#F2F2F2,stroke:#666666
    Validator["Validator"]
    style Validator fill:#FFF2CC,stroke:#D6B656
end

subgraph StakingPlatform["Staking Platform (e.g., Lido)"]
    style StakingPlatform fill:#C2E0C6,stroke:#5FB483
    StakingPool["Staking Pool"]
    style StakingPool fill:#E6F2FF,stroke:#B8D8F2
    stETH["stETH (Liquid Staking Token)"]
    style stETH fill:#D5E8D4,stroke:#82B366
end

subgraph RestakingPlatform["Restaking Platform (e.g., EigenLayer)"]
    style RestakingPlatform fill:#E1D5E7,stroke:#9673A6
    RestakingContract
    style RestakingContract fill:#FFDDCC,stroke:#E69966
    OtherProtocol["Other Protocol/Service"]
    style OtherProtocol fill:#F08080,stroke:#DC143C
end

Validator -- "Stake 32 ETH" --> StakingPool
StakingPool -- "Issues" --> stETH
Validator -- "Receives" --> stETH

stETH -- "Restake" --> RestakingContract
RestakingContract -- "Secures" --> OtherProtocol
RestakingContract -- "Provides" --> AdditionalRewards["Additional Rewards"]

Validator -- "Earns" --> StakingRewards["Staking Rewards (ETH)"]

```


**Restaking** : allows Ethereum validators to use their staked ETH to secure additional protocols, like EigenLayer, beyond the main Ethereum network. This increases security for multiple protocols and enables validators to earn additional rewards. Validators' staked ETH is verified and utilized across different applications, enhancing capital efficiency and network robustness.

## Smart Contracts

```mermaid
graph LR
    subgraph SmartContract["Smart Contract"]
        style SmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["Contract Code<br/>(Solidity, etc.)"]
        StateVariables["State Variables"]
        Functions["Functions"]
        Events["Events"]
    end

    subgraph Blockchain["Blockchain"]
        style Blockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork
    end

    subgraph Participants["Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        User1["User 1"]
        style User1 fill:#C2E0C6,stroke:#5FB483
        User2["User 2"]
        style User2 fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Trigger["Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        ExternalCall["External Call<br/>(User/Contract)"]
        style ExternalCall fill:#E6F2FF,stroke:#B8D8F2
        TimeBased["Time-Based<br/>(e.g., Scheduled Payment)"]
        style TimeBased fill:#E6F2FF,stroke:#B8D8F2
        EventBased["Event-Based<br/>(e.g., Price Change)"]
        style EventBased fill:#E6F2FF,stroke:#B8D8F2
    end


    User1 -- "Interacts With" --> SmartContract
    User2 -- "Interacts With" --> SmartContract

    SmartContract -- "Deployed On" --> BlockchainNetwork
    ContractCode -- "Defines" --> StateVariables
    ContractCode -- "Defines" --> Functions
    ContractCode -- "Defines" --> Events
    Functions -- "Updates" --> StateVariables
    Events -- "Emitted When State Changes" --> BlockchainNetwork

    ExternalCall --> Functions
    TimeBased --> Functions
    EventBased --> Functions

```

**Smart contract**: A smart contract is a self-executing contract with the terms directly written into code on a blockchain. It automatically enforces and executes the terms of an agreement when predefined conditions are met. This eliminates the need for intermediaries, reducing costs and enhancing security. Smart contracts are commonly used in various applications, including finance, real estate, and supply chain management.

## Dapp (Decentralized Application)

```mermaid
flowchart TB
    SmartContract["Smart Contract: MyToken"]
    Blockchain["Blockchain"]
    TransactionLog["Transaction Log (Blockchain)"]
    EventData["Event Data (Blockchain)"]
    BalancesData["Balances Data (Blockchain)"]
    dApp["Decentralized Application"]
    Web3["Web3.js/Ethers.js"]

    SmartContract -->|Deploys to| Blockchain
    SmartContract -->|Emits Event| TransactionLog
    SmartContract -->|Reads/Writes| BalancesData
    Blockchain -->|Stores| TransactionLog
    Blockchain -->|Stores| EventData
    Blockchain -->|Stores| BalancesData
    dApp -->|Interacts with| Web3
    Web3 -->|Reads Event Data| EventData
    Web3 -->|Reads/Writes| BalancesData
    Web3 -->|Listens for Events| TransactionLog

    subgraph BlockchainSection[" "]
        Blockchain
        TransactionLog
        EventData
        BalancesData
    end

    classDef blockchain fill:#ff9,stroke:#333,stroke-width:2px;
    classDef contract fill:#99f,stroke:#333,stroke-width:2px;
    classDef external fill:#9f9,stroke:#333,stroke-width:2px;

    Blockchain:::blockchain
    TransactionLog:::blockchain
    EventData:::blockchain
    BalancesData:::blockchain
    dApp:::external
    Web3:::external
    SmartContract:::contract

```

Decentralized applications (dApps) are software applications that run on a blockchain or peer-to-peer network of computers instead of a single computer. They are open-source, operate autonomously, and have their data stored immutably on a blockchain. dApps are often used to create financial services, social networks, games, and other applications that don't require a central authority or intermediary.
Examples of dApps : Uniswap, Compound, Aave, MakerDAO etc.

## NFT (Non-Fungible Token)

```mermaid
graph LR
    subgraph NFTSmartContract["NFT Smart Contract"]
        style NFTSmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["Contract Code<br/>(Solidity, etc.)"]
        StateVariables["State Variables<br/>(Token Metadata, Owner)"]
        Functions["Functions<br/>(Mint, Transfer, Burn)"]
        Events["Events<br/>(Transfer, Approval)"]
    end

    subgraph EthereumBlockchain["Ethereum Blockchain"]
        style EthereumBlockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork["Ethereum Network"]
    end

    subgraph Participants["Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Creator["Creator"]
        style Creator fill:#C2E0C6,stroke:#5FB483
        Buyer["Buyer"]
        style Buyer fill:#F5DEB3,stroke:#D2B48C
        Seller["Seller"]
        style Seller fill:#ADD8E6,stroke:#4682B4
    end
    
    subgraph Trigger["Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        MintCall["Mint Call<br/>(Creator)"]
        style MintCall fill:#E6F2FF,stroke:#B8D8F2
        TransferCall["Transfer Call<br/>(Owner)"]
        style TransferCall fill:#E6F2FF,stroke:#B8D8F2
        BurnCall["Burn Call<br/>(Owner)"]
        style BurnCall fill:#E6F2FF,stroke:#B8D8F2
    end

    Creator -- "Interacts With" --> NFTSmartContract
    Buyer -- "Interacts With" --> NFTSmartContract
    Seller -- "Interacts With" --> NFTSmartContract

    NFTSmartContract -- "Deployed On" --> BlockchainNetwork
    ContractCode -- "Defines" --> StateVariables
    ContractCode -- "Defines" --> Functions
    ContractCode -- "Defines" --> Events
    Functions -- "Updates" --> StateVariables
    Events -- "Emitted When State Changes" --> BlockchainNetwork

    MintCall --> Functions
    TransferCall --> Functions
    BurnCall --> Functions

```

**An NFT (Non-Fungible Token)** is a unique digital asset represented by a smart contract. The smart contract contains code that defines state variables like token metadata and ownership, and functions for minting, transferring, and burning NFTs. Creators can mint new NFTs, owners can transfer them, and events like transfers and approvals are logged on the blockchain. Participants such as creators, buyers, and sellers interact with the NFT smart contract to manage the lifecycle of NFTs, ensuring transparency and security in ownership.
##  Lending and Borrowing

```mermaid
graph LR
    subgraph DeFiSmartContract["DeFi Lending/Borrowing Smart Contract"]
        style DeFiSmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["Contract Code<br/>(Solidity, etc.)"]
        StateVariables["State Variables<br/>(Lender Balances, Borrower Debts)"]
        Functions["Functions<br/>(Deposit, Withdraw, Borrow, Repay)"]
        Events["Events<br/>(Deposit, Withdrawal, Borrow, Repay)"]
    end

    subgraph EthereumBlockchain["Ethereum Blockchain"]
        style EthereumBlockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork["Ethereum Network"]
    end

    subgraph Participants["Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Lender["Lender"]
        style Lender fill:#C2E0C6,stroke:#5FB483
        Borrower["Borrower"]
        style Borrower fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Trigger["Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        DepositCall["Deposit Call<br/>(Lender)"]
        style DepositCall fill:#E6F2FF,stroke:#B8D8F2
        WithdrawCall["Withdraw Call<br/>(Lender)"]
        style WithdrawCall fill:#E6F2FF,stroke:#B8D8F2
        BorrowCall["Borrow Call<br/>(Borrower)"]
        style BorrowCall fill:#E6F2FF,stroke:#B8D8F2
        RepayCall["Repay Call<br/>(Borrower)"]
        style RepayCall fill:#E6F2FF,stroke:#B8D8F2
    end

    Lender -- "Interacts With" --> DeFiSmartContract
    Borrower -- "Interacts With" --> DeFiSmartContract

    DeFiSmartContract -- "Deployed On" --> BlockchainNetwork
    ContractCode -- "Defines" --> StateVariables
    ContractCode -- "Defines" --> Functions
    ContractCode -- "Defines" --> Events
    Functions -- "Updates" --> StateVariables
    Events -- "Emitted When State Changes" --> BlockchainNetwork

    DepositCall --> Functions
    WithdrawCall --> Functions
    BorrowCall --> Functions
    RepayCall --> Functions

```

**Lenders** deposit funds into a smart contract to earn interest. **Borrowers** take out loans by using collateral and paying interest. Smart contracts manage balances, debts, and ensure transparent transactions. Lenders can withdraw their funds, and borrowers can repay loans through specific function calls. This system enhances accessibility and efficiency in financial services without intermediaries.

## Liquidity Pool

```mermaid
graph LR;

subgraph LiquidityPool["Liquidity Pool"]
    TokenA["Token A"]:::skyblue
    TokenB["Token B"]:::limegreen
    AMM["Automated\nMarket Maker"]:::gold
end

User1["User 1\n(Liquidity\nProvider)"]:::coral -->|Deposits\nToken A & B| LiquidityPool
User2["User 2\n(Liquidity\nProvider)"]:::orchid -->|Deposits\nToken A & B| LiquidityPool
LiquidityPool -->|Swaps\nToken A <--> B| User3["User 3\n(Swapper)"]:::hotpink
LiquidityPool -->|Trading\nFees| User1 & User2
AMM -->|Determines\nPrice & Executes\nTrades| LiquidityPool

classDef aqua fill:#00FFFF,stroke:#333,stroke-width:2px;
classDef skyblue fill:#87CEEB,stroke:#333,stroke-width:2px;
classDef limegreen fill:#32CD32,stroke:#333,stroke-width:2px;
classDef coral fill:#FF7F50,stroke:#333,stroke-width:2px;
classDef orchid fill:#DA70D6,stroke:#333,stroke-width:2px;
classDef hotpink fill:#FF69B4,stroke:#333,stroke-width:2px;
classDef gold fill:#FFD700,stroke:#333,stroke-width:2px;

```
**Liquidity Pool**:

- Core component of decentralized exchanges (DEX) like Uniswap.
- A smart contract holding a pair of tokens (e.g., Token A and Token B).
- The ratio of tokens in the pool determines their relative prices.

**Automated Market Maker (AMM)**:

- Algorithm that governs the liquidity pool.
- Determines token prices based on the ratio of assets in the pool using a constant product formula.
- Automatically executes trades when users interact with the pool.

**Liquidity Providers (User 1 & User 2)**:

- Deposit equal value of two tokens into the pool.
- Receive LP tokens representing their share of the pool.
- Earn trading fees proportional to their share.
- Face risk of impermanent loss if token prices diverge significantly.

**Swappers (User 3)**:

- Trade one token for another directly within the pool.
- Pay a small transaction fee, distributed to liquidity providers.


## ETH VM ( Virtual Machine)

This is a virtual computer that runs on the Ethereum network. It's responsible for executing smart contracts.

```mermaid
graph LR
    subgraph EVM["Ethereum Virtual Machine (EVM)"]
        style EVM fill:#F2F2F2,stroke:#666666
        Bytecode["Bytecode<br/>(Smart Contract Instructions)"]
        style Bytecode fill:#FFF2CC,stroke:#D6B656
        EVMRuntime["EVM Runtime Environment"]
        style EVMRuntime fill:#C2E0C6,stroke:#5FB483
        Storage["Storage<br/>(Persistent Data)"]
        style Storage fill:#E1D5E7,stroke:#9673A6
        Memory["Memory<br/>(Temporary Data)"]
        style Memory fill:#D5E8D4,stroke:#82B366
        Stack["Stack<br/>(Computation)"]
        style Stack fill:#FFDDCC,stroke:#E69966
    end

    subgraph External["External"]
        Blockchain["Ethereum Blockchain"]
        style Blockchain fill:#F0F0F0,stroke:#888888
    end

    Bytecode -- "Executes" --> EVMRuntime
    EVMRuntime -- "Reads/Writes" --> Storage
    EVMRuntime -- "Reads/Writes" --> Memory
    EVMRuntime -- "Uses" --> Stack
    EVMRuntime -- "Interacts With" --> Blockchain

    Blockchain -- "Stores" --> Bytecode
    Blockchain -- "Provides" --> Gas["Gas (Execution Fee)"]
    Gas --> EVMRuntime

```


## TVL (Total Value Locked)

```mermaid
graph TD
    subgraph DeFiProtocol["DeFi Protocol (e.g., Uniswap, Aave)"]
        style DeFiProtocol fill:#F2F2F2,stroke:#666666
        LiquidityPools["Liquidity Pools"]
        style LiquidityPools fill:#FFF2CC,stroke:#D6B656
        LendingPools["Lending Pools"]
        style LendingPools fill:#E1D5E7,stroke:#9673A6
        StakingContracts["Staking Contracts"]
        style StakingContracts fill:#D5E8D4,stroke:#82B366
    end

    subgraph Assets["Assets (Locked in the Protocol)"]
        style Assets fill:#C2E0C6,stroke:#5FB483
        Crypto1["Crypto Asset 1 (e.g., ETH)"]
        Crypto2["Crypto Asset 2 (e.g., USDC)"]
        Crypto3["Crypto Asset 3 (e.g., WBTC)"]
        LPTokens["LP Tokens (Liquidity Provider Tokens)"]
    end

    subgraph TVL["Total Value Locked (TVL)"]
        style TVL fill:#FFDDCC,stroke:#E69966
        TVLValue["USD Value of All<br/>Locked Assets"]
    end

    Assets --> LiquidityPools
    Assets --> LendingPools
    Assets --> StakingContracts
    LiquidityPools --> TVLValue
    LendingPools --> TVLValue
    StakingContracts --> TVLValue

    TVLValue -- "Indicates" --> ProtocolPopularity["Protocol Popularity"]
    TVLValue -- "Reflects" --> UserTrust["User Trust & Confidence"]
    TVLValue -- "Influences" --> YieldRates["Yield Rates & Incentives"]

```

## Total/Locked/Circulating Supply

```mermaid
graph
    subgraph TotalSupply["Total Supply"]
        style TotalSupply fill:#FFF2CC,stroke:#D6B656
        AllCoins["All coins/tokens ever created"]
    end

    subgraph CirculatingSupply["Circulating Supply"]
        style CirculatingSupply fill:#C2E0C6,stroke:#5FB483
        AvailableCoins["Coins/tokens available<br/>for trading and use"]
        
    end
    
    subgraph LockedSupply["Locked Supply"]
        style LockedSupply fill:#E1D5E7,stroke:#9673A6
        TeamHoldings["Tokens held by the project team/investors"]
        Vesting["Tokens locked in vesting schedules"]
        Reserve["Tokens held in reserve funds"]
    end

    TotalSupply --> LockedSupply
    TotalSupply --> CirculatingSupply

    subgraph Factors["Factors Affecting Circulating Supply"]
        style Factors fill:#D4EDDA,stroke:#4CAF50
        NewIssuance["New Issuance (Mining/Minting)"]
        CoinBurn["Coin Burning"]
        MarketSentiment["Market Sentiment"]
    end

    NewIssuance --> CirculatingSupply
    CoinBurn --> CirculatingSupply
    MarketSentiment --> CirculatingSupply
    CirculatingSupply -- "Impacts" --> MarketCap["Market Capitalization<br/>(Price x Circulating Supply)"]
    CirculatingSupply -- "Impacts" --> CoinPrice["Coin/Token Price"]

```
- **Total Supply**: The maximum number of coins or tokens that will ever exist for a particular cryptocurrency. This number is often fixed or predetermined.

- **Locked Supply**:  The portion of the total supply that is not currently available for trading or use. This includes tokens held by the project team, investors, or in reserve funds, often subject to vesting schedules (gradual release over time).

- **Circulating Supply**:  The number of coins or tokens that are currently available and actively traded on the market. This is the supply that directly impacts the market dynamics of the cryptocurrency.

## TPS ( Transactions Per Second)

```mermaid
graph 
    TPS["Transactions Per Second (TPS)"]
    style TPS fill:#F2F2F2,stroke:#666666

    subgraph Factors["Factors Influencing TPS"]
        style Factors fill:#D4EDDA,stroke:#4CAF50
        BlockSize["Block Size"]
        style BlockSize fill:#FFF9E6,stroke:#F9EBC8
        BlockTime["Block Time"]
        style BlockTime fill:#FFF9E6,stroke:#F9EBC8
        Consensus["Consensus Mechanism"]
        style Consensus fill:#FFF9E6,stroke:#F9EBC8
        TxComplexity["Transaction Complexity"]
        style TxComplexity fill:#FFF9E6,stroke:#F9EBC8
    end

    subgraph Benefits["Benefits of High TPS"]
        style Benefits fill:#C2E0C6,stroke:#5FB483
        Scalability["Scalability"]
        LowFees["Low Fees"]
        FastTransactions["Fast Transactions"]
        BetterUX["Better User Experience"]
    end

    subgraph Drawbacks["Drawbacks of High TPS"]
        style Drawbacks fill:#F2DEDE,stroke:#D9534F
        Centralization["Centralization Risk"]
        Security["Potential Security Risks"]
        HighCosts["Higher Costs"]
        DDOSRisk["DDoS Attack Risk"]
    end

    subgraph Examples["Examples of TPS in Blockchains"]
        style Examples fill:#E1D5E7,stroke:#9673A6
        Bitcoin["Bitcoin (BTC) ~4-7 TPS"]
        Ethereum["Ethereum (ETH) ~15-45 TPS"]
        Solana["Solana (SOL) ~50,000 TPS (theoretical)"]
    end

    TPS --> Benefits
    TPS --> Drawbacks
    BlockSize --> TPS
    BlockTime --> TPS
    Consensus --> TPS
    TxComplexity --> TPS
    TPS --> Examples

```
**TPS**: measures a blockchain's transaction processing speed, influenced by block size, time, consensus, and complexity.
High TPS offers scalability and fast transactions, but can risk security and decentralization.

## Layer 2

```mermaid
graph LR
    subgraph ChildChain["Child Chain"]
        style ChildChain fill:#95E1D3,stroke:#00868B
        ChildBlock1["Block 1"]
        ChildBlock2["Block 2"]
        ChildBlock3["Block 3"]
        ChildBlock1 --> ChildBlock2 --> ChildBlock3
    end

    subgraph ParentChain["Parent Chain"]
        style ParentChain fill:#DCDCDC,stroke:#696969
        ParentBlock1["Block 1"]
        ParentBlock2["Block 2"]
        ParentBlock3["Block 3"]
        ParentBlock1 --> ParentBlock2 --> ParentBlock3
    end

    ChildBlock3 --> Checkpoint["Checkpoint"]
    Checkpoint --> ParentBlock2
```
### Rollup

```mermaid
graph LR
    subgraph RollupChain["Rollup Chain"]
        style RollupChain fill:#FFDDC1,stroke:#FFB6C1
        RollupBlock1["Block 1"]
        RollupBlock2["Block 2"]
        RollupBlock3["Block 3"]
        RollupBlock1 --> RollupBlock2 --> RollupBlock3
    end

    subgraph MainChain["Main Chain"]
        style MainChain fill:#DCDCDC,stroke:#696969
        MainBlock1["Block 1"]
        MainBlock2["Block 2"]
        MainBlock3["Block 3"]
        MainBlock1 --> MainBlock2 --> MainBlock3
    end

    RollupBlock3 --> RollupBatch["Batch of Transactions"]
    RollupBatch --> MainBlock2
    
    RollupTypes["Types of Rollups"] --> OptimisticRollups["Optimistic Rollups"]
    RollupTypes --> ZKRollups["ZK Rollups"]
    OptimisticRollups --> RollupChain
    ZKRollups --> RollupChain
```


## Sharding

```mermaid
graph LR
    subgraph BeaconChain["Beacon Chain"]
        style BeaconChain fill:#F2F2F2,stroke:#666666
        BeaconNode1["Beacon Node 1"]
        BeaconNode2["Beacon Node 2"]
        BeaconNode3["Beacon Node 3"]
        BeaconNode1 --> BeaconNode2
        BeaconNode2 --> BeaconNode3
    end

    subgraph ShardChains["Shard Chains"]
        style ShardChains fill:#C2E0C6,stroke:#5FB483
        ShardChain1["Shard Chain 1"]
        ShardChain2["Shard Chain 2"]
        ShardChain3["Shard Chain 3"]
    end

    BeaconChain --> ShardChain1
    BeaconChain --> ShardChain2
    BeaconChain --> ShardChain3

    subgraph ShardChain1
        style ShardChain1 fill:#E6F2FF,stroke:#B8D8F2
        ShardBlock1["Shard Block 1"]
        ShardBlock2["Shard Block 2"]
        ShardBlock1 --> ShardBlock2
    end

    subgraph ShardChain2
        style ShardChain2 fill:#E6F2FF,stroke:#B8D8F2
        ShardBlock3["Shard Block 3"]
        ShardBlock4["Shard Block 4"]
        ShardBlock3 --> ShardBlock4
    end

    subgraph ShardChain3
        style ShardChain3 fill:#E6F2FF,stroke:#B8D8F2
        ShardBlock5["Shard Block 5"]
        ShardBlock6["Shard Block 6"]
        ShardBlock5 --> ShardBlock6
    end
```

- **Beacon Chain** (Coordinator): This is the main chain that manages the entire sharded network. It assigns validators to different shard chains and ensures consensus across the network.

- **Shard Chains** (Parallel Processing): These are smaller chains that run in parallel, each processing a portion of the network's transactions and state. This parallel processing significantly increases the overall transaction throughput of the blockchain.

- **Shard Blocks**: Blocks on individual shard chains contain transactions and data relevant to that specific shard.
