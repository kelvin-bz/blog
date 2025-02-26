---
title: "Blockchain for dummies" 
categories: [blockchain]
date: 2024-07-06 00:00:00
image: "/assets/images/blockchain-for-dummies.png"
tags: [blockchain]

---

In this article, we will cover the basics of blockchain technology in a simple and easy-to-understand way. Each section start with a simple diagram to explain the concept and then followed by a brief explanation. The good part of diagram is help you to understand the concept in a visual way and of course more questions will come to your mind, find the answers by yourself and making your own research is the best way to learn and memorize the concept.


## Transaction - Block

```mermaid
graph LR

    subgraph Keys["fa:fa-key Keys"]
        style Keys fill:#C2E0C6,stroke:#5FB483
        PrivateKey["fa:fa-lock Private Key (Sender)"]
        PublicKey["fa:fa-unlock Public Key (Sender/Recipient)"]
    end
    PrivateKey --"fa:fa-pen Signs"--> TransactionSignature
    PublicKey --"fa:fa-check Verifies"--> TransactionSignature

    subgraph Transaction["fa:fa-file-alt Transaction"]
      style Transaction fill:#E1D5E7,stroke:#9673A6
      TransactionSignature["fa:fa-signature Digital Signature"]
      subgraph TransactionData["fa:fa-database Transaction Data"]
        style TransactionData fill:#FFF2CC,stroke:#D6B656
        TransactionDetails["fa:fa-info-circle Transaction Details<br/>(Sender, Recipient, Amount)"]
    end
    end

    subgraph Verification["fa:fa-shield-alt Verification"]
      style Verification fill:#D4EDDA,stroke:#4CAF50
      BlockchainNetwork["fa:fa-network-wired Blockchain Network"]
    end

    Transaction --"fa:fa-check Verified by"--> BlockchainNetwork

    subgraph Block["fa:fa-cube Block (Example)"]
        style Block fill:#F0F0F0,stroke:#888888
        BlockHeader["fa:fa-header Block Header"]
        BlockData["fa:fa-database Block Data (Multiple Transactions)"]
    end

    Transaction -.-> BlockData
    BlockHeader --> BlockHash["fa:fa-fingerprint Block Hash"]
    BlockHeader --> PreviousBlockHash["fa:fa-link Previous Block's Hash"]
    BlockHeader --> Nonce["fa:fa-random Nonce"]

```

- **Transaction**: A digital record of value transfer between two parties (sender and recipient) with a specific amount. It's signed with the sender's private key and verified by the network using their public key. In the case of deploying a smart contract, the transaction does not have a recipient address. [ethereum's doc](https://ethereum.org/en/developers/docs/transactions/)


- **Block**: A collection of verified transactions grouped together, along with a header containing metadata (e.g., timestamp, previous block's hash).


## Miner - Node - Wallet

```mermaid
graph LR
    subgraph Network["fa:fa-network-wired Blockchain Network"]
        style Network fill:#F2F2F2,stroke:#666666
        Transactions["fa:fa-exchange-alt Transactions"]
        style Transactions fill:#E1D5E7,stroke:#9673A6
        Blocks["fa:fa-cube Blocks"]
        style Blocks fill:#F0F0F0,stroke:#888888
    end

    subgraph Participants["fa:fa-users Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Wallet["fa:fa-wallet Wallet (User)"]
        style Wallet fill:#FFF2CC,stroke:#D6B656
        Node["fa:fa-server Node"]
        style Node fill:#C2E0C6,stroke:#5FB483
        Miner["fa:fa-user-shield Miner"]
        style Miner fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Wallet
      PublicKey["fa:fa-key Public Key (Sender/Recipient)"]
      PrivateKey["fa:fa-lock Private Key (Sender)"]
    end

    Wallet -- "fa:fa-paper-plane Creates/Receives" --> Transactions
    Node -- "fa:fa-check Validates/Stores" --> Transactions
    Node -- "fa:fa-check Validates/Stores" --> Blocks
    Miner -- "fa:fa-hammer Creates/Validates" --> Blocks
    Transactions -.-> Blocks

    PrivateKey -- "fa:fa-pen Signs" --> Transactions
    PublicKey -- "fa:fa-check Verifies" --> Transactions
    Miner -- "fa:fa-coins Receives Rewards From" --> Blocks


```

- **Miner**: Miners solve complex puzzles to add new blocks to the blockchain, ensuring its security and earning rewards in the process.

- **Node**: Nodes maintain copies of the blockchain, validate transactions, and relay information across the network.

- **Wallet**:  Wallets store users' private keys (used for signing transactions) and public keys (used for receiving transactions), allowing them to interact with the blockchain.

## Mining Pool

```mermaid
graph TD
    subgraph miners["fa:fa-users Miners"]
        miner1["fa:fa-user Miner 1"]
        style miner1 fill:#FFFACC,stroke:#FFD700
        miner2["fa:fa-user Miner 2"]
        style miner2 fill:#FFFACD,stroke:#FFD700
        miner3["fa:fa-user Miner 3"]
        style miner3 fill:#FFFACD,stroke:#FFD700
    end
    
    miner1 --> |"Computing Power"|poolServer
    miner2 --> |"Computing Power"|poolServer
    miner3 --> |"Computing Power"|poolServer

    poolServer["fa:fa-database Pool Server"]
    style poolServer fill:#AFEEEE,stroke:#40E0D0,stroke-width:2px

    poolServer --> |"Solution Found"| blockchain["fa:fa-link Blockchain"]
    style blockchain fill:#98FB98,stroke:#32CD32,stroke-width:2px
    blockchain --> |"Block Reward"| poolServer
    poolServer --> |"Proportional Rewards"| miners

```

A mining pool is a collaborative effort where multiple cryptocurrency miners combine their computational resources to increase the chances of successfully mining a block and earning rewards. The reward is distributed proportionally to the miners based on the number of shares they contributed. 

##  Immutability and Consensus

```mermaid
graph TD
    block1["fa:fa-cube Block 1"]
    block2["fa:fa-cube Block 2"]
    block3["fa:fa-times-circle Block 3"]:::invalid
    block4["fa:fa-times-circle Block 4"]:::invalid
    block5["fa:fa-times-circle Block 5"]:::invalid

    block1 -->|fa:fa-link Hash Link| block2
    block2 -->|fa:fa-link Hash Link| block3
    block3 -->|fa:fa-link Hash Link| block4
    block4 -->|fa:fa-link Hash Link| block5

    block2 -- "fa:fa-edit Data Change" --> invalidBlock2["fa:fa-ban Invalid Block 2"]
    invalidBlock2 -.-> |"fa:fa-times Invalidates"| block3
    invalidBlock2 -.-> |"fa:fa-times Invalidates"| block4
    invalidBlock2 -.-> |"fa:fa-times Invalidates"| block5

    classDef invalid fill:#f96


```

**Immutability**:This means that once a block of data is added to the blockchain, it is extremely difficult to alter or remove. This immutability is crucial for maintaining trust and security in the network. Each block in a blockchain contains a unique cryptographic hash that is calculated based on the block's contents and the hash of the previous block. If you change any data within a block, its hash changes, invalidating all subsequent blocks in the chain.

```mermaid
graph LR
    classDef blockchain fill:#ff9,stroke:#333,stroke-width:2px;
    
    subgraph Blockchain["fa:fa-network-wired Blockchain"]
        style Blockchain fill:#ff9,stroke:#333,stroke-width:2px
        Block1["fa:fa-cube Block 1"]
        Block2["fa:fa-cube Block 2"]
        Block3["fa:fa-cube Block 3"]

        Block1 --> |"fa:fa-link Hash Link"| Block2
        Block2 --> |"fa:fa-link Hash Link"| Block3
    end

    Block2 -- "fa:fa-edit Attempted Data Change" --> Disagreement["fa:fa-exclamation-triangle Disagreement with\nNetwork Consensus"]


```

**Consensus**: Changes to the blockchain require the agreement (consensus) of the majority of the network's nodes. To modify a historical block, you'd need to convince the majority of nodes to rewrite the entire chain from that point onwards, which is computationally infeasible and extremely unlikely.

Different blockchains use various consensus mechanisms to agree on the validity of transactions and the state of the blockchain. 

**Proof of Work (PoW)**: Used by Bitcoin, it requires solving complex puzzles to add blocks, making it difficult for malicious actors to alter history due to the immense computational power needed.

**Proof of Stake (PoS)**: Employed by Ethereum 2.0, it selects validators based on staked cryptocurrency, disincentivizing malicious actions as they risk losing their stake.



## P2P (Peer-to-Peer) Network

A peer-to-peer (P2P) network is a type of network architecture where all participants (devices or computers) have equal status and capabilities.  Unlike traditional client-server models, there's no central authority governing the network. Instead, each participant, known as a node, directly communicates and shares resources with other nodes.

```mermaid
graph TD
    classDef network fill:#f0f0f0,stroke:#333,stroke-width:2px;

    subgraph P2PBlockchainNetwork["fa:fa-sitemap Peer-to-Peer Blockchain Network"]
        style P2PBlockchainNetwork fill:#f0f0f0,stroke:#333,stroke-width:2px
        A["fa:fa-server Node 1"]
        B["fa:fa-server Node 2"]
        C["fa:fa-server Node 3"]
        A <--"fa:fa-share-alt Share Ledger Data & Validate Transactions"--> B
        A <--> C
        B <--> C
    end
```


## Hashrate

```mermaid
graph LR
    HashRate["fa:fa-tachometer-alt BTC Hash Rate<br/>614.42 EH/s"]
    style HashRate fill:#F2F2F2,stroke:#666666
    HashRate -- "fa:fa-info Represents" --> TotalNetworkPower["fa:fa-cogs Total Network Computing Power"]
    style TotalNetworkPower fill:#FFDDCC,stroke:#E69966

    TotalNetworkPower -- "fa:fa-users Influenced by" --> NumberOfMiners["fa:fa-user-friends Number of Miners"]
    style NumberOfMiners fill:#FFF2CC,stroke:#D6B656

    TotalNetworkPower -- "fa:fa-microchip Influenced by" --> MiningHardware["fa:fa-tools Mining Hardware Efficiency"]
    style MiningHardware fill:#C2E0C6,stroke:#5FB483

    HashRate -- "fa:fa-tasks Determines" --> Difficulty["fa:fa-dumbbell Mining Difficulty"]
    style Difficulty fill:#E1D5E7,stroke:#9673A6

    HashRate -- "fa:fa-clock Affects" --> BlockTime["fa:fa-hourglass-half Average Block Time<br/>~10 minutes"]
    style BlockTime fill:#D5E8D4,stroke:#82B366
    
    HashRate -- "fa:fa-lock Indicates" --> Security["fa:fa-shield-alt Network Security"]
    style Security fill:#9673A6,stroke:#E1D5E7

```

**Bitcoin Hashrate** measures the total computing power miners use to secure the network, currently at 614.42 EH/s. A higher hashrate enhances security by making it harder for bad actors to control the network. It also influences mining difficulty, ensuring consistent block creation times.

## ETH (proof of stake)

```mermaid
graph LR
    subgraph Network["fa:fa-network-wired Blockchain Network (Ethereum 2.0)"]
        style Network fill:#F2F2F2,stroke:#666666
        Transactions["fa:fa-exchange-alt Transactions"]
        style Transactions fill:#E1D5E7,stroke:#9673A6
        Blocks["fa:fa-cube Blocks"]
        style Blocks fill:#F0F0F0,stroke:#888888
    end

    subgraph Participants["fa:fa-users Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Wallet["fa:fa-wallet Wallet (User)"]
        style Wallet fill:#FFF2CC,stroke:#D6B656
        Node["fa:fa-server Node"]
        style Node fill:#C2E0C6,stroke:#5FB483
        Validator["fa:fa-user-shield Validator"]
        style Validator fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Wallet
      PublicKey["fa:fa-key Public Key (Sender/Recipient)"]
      PrivateKey["fa:fa-lock Private Key (Sender)"]
    end

    Wallet -- "fa:fa-paper-plane Creates/Receives" --> Transactions
    Node -- "fa:fa-check Validates/Stores" --> Transactions
    Node -- "fa:fa-check Validates/Stores" --> Blocks
    Validator -- "fa:fa-gavel Proposes/Validates" --> Blocks
    Transactions -.-> Blocks

    PrivateKey -- "fa:fa-pen Signs" --> Transactions
    PublicKey -- "fa:fa-check Verifies" --> Transactions
    Validator -- "fa:fa-coins Receives Rewards From" --> Blocks
    Validator --> StakedETH["fa:fa-piggy-bank Staked ETH (32 ETH)"]

    

```

**POS (proof of stake)**: stake ETH to become validators. Validators are randomly selected to propose new blocks, with others attesting to their validity. Correct validators earn rewards, while incorrect ones are penalized. This replaces mining, making Ethereum more scalable and eco-friendly.

## Gas Fees
Gas in the Ethereum network is a unit that measures the computational work required to execute transactions or smart contracts. Each operation on the Ethereum Virtual Machine (EVM) requires a certain amount of gas, which must be paid for in Ether (ETH). Gas ensures that transactions and contract executions are fairly priced according to their resource consumption. Users specify a gas limit and a gas price for their transactions; if the gas limit is exceeded, the transaction fails but the gas used is still deducted. This mechanism prevents network abuse and incentivizes efficient code execution. 

```mermaid
graph TD
    subgraph UserA["fa:fa-user Bob's Account"]
        style UserA fill:#9f9,stroke:#333,stroke-width:2px
        BalanceA["fa:fa-money-bill-wave 1.0042 ETH"]
    end

    style Blockchain fill:#ff9,stroke:#333,stroke-width:2px
    Miner["fa:fa-gavel Miner/Validator"]
    
    subgraph UserB["fa:fa-user Alice's Account"]
        style UserB fill:#9f9,stroke:#333,stroke-width:2px
        BalanceB["fa:fa-money-bill-wave 0 ETH"]
    end

    UserA --"fa:fa-arrow-right Sends 1 ETH + 0.0042 ETH Fee"--> Blockchain
    Blockchain --"fa:fa-arrow-right Transfers 1 ETH"--> UserB
    Blockchain --"fa:fa-coins Pays 0.00021 ETH Tip"--> Miner
    Blockchain --"fa:fa-fire Burns 0.00399 ETH"--> LostETH["fa:fa-money-bill-wave 0.00399 ETH"]

```

- Transaction Cost: Sending ETH has a base cost of 21,000 units of gas.
- Gas Price: Bob sets a gas price of 200 gwei (190 gwei base fee + 10 gwei tip).
- Total Fee: The total fee Bob pays is 21,000 gas * 200 gwei/gas = 4,200,000 gwei (or 0.0042 ETH).
- Bob's account is debited 1.0042 ETH (1 ETH for Alice + 0.0042 ETH fee).
- Alice's account is credited 1 ETH.
0.00399 ETH is burned (removed from circulation).
- The miner receives 0.00021 ETH as a tip.

### Unit Conversion



| Unit    | Symbol | Value in Wei (Smallest Unit) | Usage                         |
| ------- | ------ | --------------------------- | ----------------------------- |
| Wei     | wei    | 1                           | Atomic unit of Ether           |
| Kwei    | Kwei   | 10^3                        | Rarely used                   |
| Mwei    | Mwei   | 10^6                        | Rarely used                   |
| Gwei    | Gwei   | 10^9                        | Gas prices, small transactions |
| Microether (Szabo) | szabo  | 10^12                        | Rarely used                   |
| Milliether (Finney) | finney  | 10^15                        | Small amounts of Ether        |
| Ether   | ETH    | 10^18                        | Standard unit for most purposes |

**Key Points:**

- **Wei:** The smallest denomination of Ether. All other units are multiples of Wei.
- **Gwei:** The most commonly used unit after Ether, especially for gas prices and small transactions.
- **Ether:** The standard unit for most transactions and interactions with Ethereum applications.

**Example Conversions:**

- 1 ETH = 1,000,000,000 Gwei
- 1 Gwei = 1,000,000,000 wei

### Gas Price and Gas Limit

```mermaid
graph LR
  subgraph transaction["fa:fa-file-invoice-dollar Transaction"]
      style transaction fill:#9f9,stroke:#333,stroke-width:2px
      data["fa:fa-database Transaction Data"]
      gasLimit["fa:fa-gas-pump Gas Limit (Maximum Allowed)"]
      gasPrice["fa:fa-dollar-sign Gas Price (Bid per Unit)"]
  end

  subgraph network["fa:fa-link Network"]
      style network fill:#ff9,stroke:#333,stroke-width:2px
      marketDemand["fa:fa-chart-line Market Demand"]
      networkCongestion["fa:fa-traffic-light Network Congestion"]
      minerPreference["fa:fa-gavel Miner Preference"]
  end

  subgraph execution["fa:fa-cogs Execution"]
      style execution fill:#f9f,stroke:#333,stroke-width:2px
      gasUsed["fa:fa-fire Gas Used (Actual Consumption)"]
      transactionFee["fa:fa-coins Transaction Fee (Total Cost)"]
  end

  transaction --> execution
  gasLimit -.-> gasUsed
  gasPrice & gasUsed --> transactionFee
  marketDemand --> gasPrice
  networkCongestion --> gasPrice
  minerPreference --> gasPrice

```
- **Gas Price as a Bid**: The maximum amount of gas (computational units) the sender is willing to spend on executing the transaction. It determines the transaction's priority and is often influenced by market demand, network congestion, and miner preference.

- **Gas Limit**: The maximum amount of gas (computational units) the sender is willing to spend on executing the transaction.

- **Gas Used**: The actual amount of gas consumed during the transaction execution.

- **Transaction Fee**: The total cost of executing the transaction, calculated as: ``Gas Used`` * ``Gas Price``

## Stablecoins

```mermaid
graph LR
    classDef cadetblue fill:#5F9EA0,stroke:#333,stroke-width:2px;
    classDef mediumaquamarine fill:#66CDAA,stroke:#333,stroke-width:2px;
    classDef lightgreen fill:#90EE90,stroke:#333,stroke-width:2px;
    classDef lightpink fill:#FFB6C1,stroke:#333,stroke-width:2px;

    subgraph Stablecoins["fa:fa-coins **Types of Stablecoins**"]
        FiatBacked["fa:fa-dollar-sign Fiat-Backed"]:::cadetblue
        CommodityBacked["fa:fa-gem Commodity-Backed"]:::mediumaquamarine
        CryptoBacked["fa:fa-b Crypto-Backed"]:::lightgreen
        Algorithmic["fa:fa-cogs Algorithmic"]:::lightpink
    end

    FiatBacked --> Tether["fa:fa-dollar-sign Tether (USDT)"]:::cadetblue
    FiatBacked --> USDCoin["fa:fa-dollar-sign USD Coin (USDC)"]:::cadetblue

    CommodityBacked --> TetherGold["fa:fa-gem Tether Gold (XAUT)"]:::mediumaquamarine
    CommodityBacked --> PAXGold["fa:fa-gem PAX Gold (PAXG)"]:::mediumaquamarine

    CryptoBacked --> Dai["fa:fa-b Dai (DAI)"]:::lightgreen

    Algorithmic --> AMPL["fa:fa-cogs Ampleforth (AMPL)"]:::lightpink
    Algorithmic --> UST["fa:fa-cogs TerraUSD (UST)"]:::lightpink


```

**Stablecoins** offer stability in the volatile crypto market, making them suitable for everyday transactions, remittances, and trading. They provide easy access to the crypto world without exposure to extreme price fluctuation. They are a bridge between traditional finance and the crypto world. 

**Fiat-Backed Stablecoins**: These stablecoins are backed 1:1 by fiat currencies like the US Dollar or Euro. The value of each coin is directly tied to the value of the fiat currency.

**Commodity-Backed Stablecoins**: These stablecoins are backed by physical assets such as precious metals. The value is tied to the commodity, providing stability based on the physical asset's value.

**Crypto-Backed Stablecoins:** These stablecoins are backed by other cryptocurrencies. They are usually over-collateralized to account for the volatility of the underlying crypto assets.

**Algorithmic Stablecoins**: These stablecoins use algorithms and smart contracts to manage the supply and stabilize the value without needing reserves. They rely on mechanisms to adjust the coin supply based on demand.

### Tether Limited

Tether Limited is the company behind USDT, one of the most widely used stablecoins in the cryptocurrency market. USDT is designed to maintain a one-to-one peg with the U.S. dollar, providing stability and making it easier for users to trade digital assets without exposure to price volatility. By bridging traditional finance with the crypto world, Tether has significantly enhanced liquidity and accessibility in the digital asset space. Its stablecoin facilitates fast, low-cost transactions across exchanges and platforms globally. Tether's innovation has played a key role in the growing adoption and mainstream acceptance of cryptocurrencies.

Tether Limited offers several stablecoin products besides USDT, including EUR₮ (Euro Tether), CNH₮ (Offshore Chinese Yuan Tether), XAU₮ (Tether Gold), GBP₮ (British Pound Sterling Tether), and MXN₮ (Mexican Peso Tether).

### USDT

USDT, commonly known as Tether, is a stablecoin cryptocurrency designed to maintain a one-to-one peg with the U.S. dollar. It offers the stability of traditional currency while harnessing the advantages of blockchain technology, enabling users to transact quickly and efficiently. USDT is widely used across various cryptocurrency exchanges, facilitating seamless trading without the volatility typically associated with digital assets.

```mermaid
graph 
    classDef primary fill:#FFFFDE,stroke:#333,stroke-width:2px;      %% Light Yellow
    classDef secondary fill:#DEFFEF,stroke:#333,stroke-width:2px;   %% Light Teal
    classDef tertiary fill:#DEDEFF,stroke:#333,stroke-width:2px;    %% Light Lavender
    classDef quaternary fill:#FFDEEF,stroke:#333,stroke-width:2px;  %% Light Pink

    subgraph Tether["fa:fa-dollar-sign Tether (USDT)"]
    style Tether fill:#fff, stroke-width:0px;    
    
    subgraph Stability["fa:fa-balance-scale Stability and Trust"]
        Pegged["fa:fa-lock Pegged to the US Dollar"]:::secondary
        Accepted["fa:fa-check-circle Widespread Acceptance"]:::secondary
    end

    subgraph Liquidity["fa:fa-water Liquidity and Volume"]
        HighLiquidity["fa:fa-tint High Liquidity"]:::tertiary
        Volume["fa:fa-chart-bar Trading Volume"]:::tertiary
    end

    subgraph Transparency["fa:fa-eye Transparency and Regulation"]
        AuditedReserves["fa:fa-file-alt Audited Reserves"]:::quaternary
        Compliance["fa:fa-shield-alt Compliance"]:::quaternary
    end

    subgraph Accessibility["fa:fa-users Accessibility and Utility"]
        EaseOfUse["fa:fa-user Ease of Use"]:::tertiary
        VersatileUtility["fa:fa-exchange-alt Versatile Utility"]:::tertiary
    end

    subgraph Integration["fa:fa-network-wired Cross-Platform Integration"]
        MultiChain["fa:fa-link Multi-Chain Support"]:::secondary
        DeFi["fa:fa-coins Integration with DeFi"]:::secondary
    end

    Stability
    Liquidity
    Transparency
    Accessibility
    Integration

    Stability --> Pegged
    Stability --> Accepted

    Liquidity --> HighLiquidity
    Liquidity --> Volume

    Transparency --> AuditedReserves
    Transparency --> Compliance

    Accessibility --> EaseOfUse
    Accessibility --> VersatileUtility

    Integration --> MultiChain
    Integration --> DeFi
    end 
```


### USDC

USDC, or USD Coin, is a stablecoin cryptocurrency designed to maintain a one-to-one peg with the U.S. dollar. Issued by Circle in collaboration with Coinbase, USDC combines the stability of fiat currency with the efficiency of blockchain technology for fast and secure transactions. It is widely supported across numerous cryptocurrency exchanges and platforms, enabling seamless trading without the volatility typically associated with digital assets. Known for its transparency, USDC's reserves are regularly audited to ensure full backing.

### DAI

DAI is a decentralized stablecoin running on the Ethereum blockchain, designed to maintain a one-to-one peg with the U.S. dollar. Unlike centralized stablecoins, DAI is generated through over-collateralized loans using cryptocurrencies as collateral via the MakerDAO protocol. This allows for a stable digital currency that is not reliant on any central authority, combining the benefits of price stability and decentralization. DAI is widely used in the decentralized finance (DeFi) ecosystem, facilitating various financial activities such as lending, borrowing, and trading without the volatility typically associated with cryptocurrencies.

## Restaking

```mermaid
graph LR
    classDef network fill:#f0f0f0,stroke:#333,stroke-width:2px;

    subgraph EthereumBlockchain["fa:fa-link Ethereum Blockchain (Beacon Chain)"]
        style EthereumBlockchain fill:#F2F2F2,stroke:#666666
        Validator["fa:fa-user-shield Validator"]
        style Validator fill:#FFF2CC,stroke:#D6B656
    end

    subgraph StakingPlatform["fa:fa-coins Staking Platform (e.g., Lido)"]
        style StakingPlatform fill:#C2E0C6,stroke:#5FB483
        StakingPool["fa:fa-piggy-bank Staking Pool"]
        style StakingPool fill:#E6F2FF,stroke:#B8D8F2
        stETH["fa:fa-coins stETH (Liquid Staking Token)"]
        style stETH fill:#D5E8D4,stroke:#82B366
    end

    subgraph RestakingPlatform["fa:fa-handshake Restaking Platform (e.g., EigenLayer)"]
        style RestakingPlatform fill:#E1D5E7,stroke:#9673A6
        RestakingContract["fa:fa-file-contract Restaking Contract"]
        style RestakingContract fill:#FFDDCC,stroke:#E69966
        OtherProtocol["fa:fa-cogs Other Protocol/Service"]
        style OtherProtocol fill:#F08080,stroke:#DC143C
    end

    Validator -- "fa:fa-arrow-right Stake 32 ETH" --> StakingPool
    StakingPool -- "fa:fa-arrow-right Issues" --> stETH
    Validator -- "fa:fa-arrow-left Receives" --> stETH

    stETH -- "fa:fa-arrow-right Restake" --> RestakingContract
    RestakingContract -- "fa:fa-shield-alt Secures" --> OtherProtocol
    RestakingContract -- "fa:fa-gift Provides" --> AdditionalRewards["fa:fa-coins Additional Rewards"]

    Validator -- "fa:fa-coins Earns" --> StakingRewards["fa:fa-coins Staking Rewards (ETH)"]


```


**Restaking** : allows Ethereum validators to use their staked ETH to secure additional protocols, like EigenLayer, beyond the main Ethereum network. This increases security for multiple protocols and enables validators to earn additional rewards. Validators' staked ETH is verified and utilized across different applications, enhancing capital efficiency and network robustness.

## Smart Contracts

```mermaid
graph LR
    subgraph SmartContract["fa:fa-file-contract Smart Contract"]
        style SmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["fa:fa-code Contract Code<br/>(Solidity, etc.)"]
        StateVariables["fa:fa-database State Variables"]
        Functions["fa:fa-cogs Functions"]
        Events["fa:fa-bullhorn Events"]
    end

    subgraph Blockchain["fa:fa-network-wired Blockchain"]
        style Blockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork["fa:fa-link Blockchain Network"]
    end

    subgraph Participants["fa:fa-users Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        User1["fa:fa-user User 1"]
        style User1 fill:#C2E0C6,stroke:#5FB483
        User2["fa:fa-user User 2"]
        style User2 fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Trigger["fa:fa-bolt Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        ExternalCall["fa:fa-external-link-alt External Call<br/>(User/Contract)"]
        style ExternalCall fill:#E6F2FF,stroke:#B8D8F2
        TimeBased["fa:fa-clock Time-Based<br/>(e.g., Scheduled Payment)"]
        style TimeBased fill:#E6F2FF,stroke:#B8D8F2
        EventBased["fa:fa-calendar Event-Based<br/>(e.g., Price Change)"]
        style EventBased fill:#E6F2FF,stroke:#B8D8F2
    end

    User1 -- "fa:fa-hand-point-right Interacts With" --> SmartContract
    User2 -- "fa:fa-hand-point-right Interacts With" --> SmartContract

    SmartContract -- "fa:fa-link Deployed On" --> BlockchainNetwork
    ContractCode -- "fa:fa-pencil-alt Defines" --> StateVariables
    ContractCode -- "fa:fa-pencil-alt Defines" --> Functions
    ContractCode -- "fa:fa-pencil-alt Defines" --> Events
    Functions -- "fa:fa-sync-alt Updates" --> StateVariables
    Events -- "fa:fa-bell Emitted When State Changes" --> BlockchainNetwork

    ExternalCall --> Functions
    TimeBased --> Functions
    EventBased --> Functions

```

**Smart contract**: A smart contract is a self-executing contract with the terms directly written into code on a blockchain. It automatically enforces and executes the terms of an agreement when predefined conditions are met. This eliminates the need for intermediaries, reducing costs and enhancing security. Smart contracts are commonly used in various applications, including finance, real estate, and supply chain management.

### Smart Contract Deploy Transaction

```mermaid
graph TD
    subgraph transaction["fa:fa-file-invoice-dollar Transaction"]
        style transaction fill:#9f9,stroke:#333,stroke-width:2px
        gasPrice["fa:fa-dollar-sign Gas Price"]
        gasLimit["fa:fa-gas-pump Gas Limit"]
        compiledBytecode["fa:fa-file-code Compiled Bytecode"]
        signature["fa:fa-signature Signature (from Private Key)"]
        noRecipient["fa:fa-ban no recipient"]
    end

    subgraph blockchain["fa:fa-link Blockchain Network"]
        style blockchain fill:#ff9,stroke:#333,stroke-width:2px
        minersValidators["fa:fa-gavel Miners/Validators"]
    end

    transaction -->|"Verified & Executed By"<i class='fas fa-arrow-right'></i>| minersValidators

```



- **No Recipient Address**: Unlike regular transactions that send funds to an address, deployment transactions don't have a recipient. The transaction itself creates the new contract address.
- **Gas Costs**: Deploying a smart contract consumes gas, and the cost depends on the complexity of your contract. Be mindful of the gas limit you set.
- **Immutability**: Once deployed, a smart contract's code is generally immutable (cannot be changed), so thorough testing is crucial before deployment.
- **Modifying a smart contract** on a blockchain like Ethereum requires deploying a new transaction with the updated code. This is due to the fundamental immutability of blockchains: once data is written to the blockchain, it cannot be changed.


## Dapp (Decentralized Application)

```mermaid
flowchart TB
    classDef blockchain fill:#ff9,stroke:#333,stroke-width:2px;
    classDef contract fill:#99f,stroke:#333,stroke-width:2px;
    classDef external fill:#9f9,stroke:#333,stroke-width:2px;

    SmartContract["fa:fa-file-contract Smart Contract: MyToken"]:::contract
    Blockchain["fa:fa-network-wired Blockchain"]:::blockchain
    TransactionLog["fa:fa-list Transaction Log (Blockchain)"]:::blockchain
    EventData["fa:fa-database Event Data (Blockchain)"]:::blockchain
    BalancesData["fa:fa-balance-scale Balances Data (Blockchain)"]:::blockchain
    dApp["fa:fa-desktop Decentralized Application"]:::external
    Web3["fa:fa-code Web3.js/Ethers.js"]:::external

    SmartContract -->|fa:fa-upload Deploys to| Blockchain
    SmartContract -->|fa:fa-bullhorn Emits Event| TransactionLog
    SmartContract -->|fa:fa-exchange-alt Reads/Writes| BalancesData
    Blockchain -->|fa:fa-save Stores| TransactionLog
    Blockchain -->|fa:fa-save Stores| EventData
    Blockchain -->|fa:fa-save Stores| BalancesData
    dApp -->|fa:fa-hand-point-right Interacts with| Web3
    Web3 -->|fa:fa-database Reads Event Data| EventData
    Web3 -->|fa:fa-exchange-alt Reads/Writes| BalancesData
    Web3 -->|fa:fa-bell Listens for Events| TransactionLog

    subgraph BlockchainSection[" "]
        Blockchain
        TransactionLog
        EventData
        BalancesData
    end


```

Decentralized applications (dApps) are software applications that run on a blockchain or peer-to-peer network of computers instead of a single computer. They are open-source, operate autonomously, and have their data stored immutably on a blockchain. dApps are often used to create financial services, social networks, games, and other applications that don't require a central authority or intermediary.
Examples of dApps : Uniswap, Compound, Aave, MakerDAO etc.

## NFT (Non-Fungible Token)

```mermaid
graph LR
    subgraph NFTSmartContract["fa:fa-file-contract NFT Smart Contract"]
        style NFTSmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["fa:fa-code Contract Code<br/>(Solidity, etc.)"]
        StateVariables["fa:fa-database State Variables<br/>(Token Metadata, Owner)"]
        Functions["fa:fa-cogs Functions<br/>(Mint, Transfer, Burn)"]
        Events["fa:fa-bullhorn Events<br/>(Transfer, Approval)"]
    end

    subgraph EthereumBlockchain["fa:fa-link Ethereum Blockchain"]
        style EthereumBlockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork["fa:fa-network-wired Ethereum Network"]
    end

    subgraph Participants["fa:fa-users Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Creator["fa:fa-user-plus Creator"]
        style Creator fill:#C2E0C6,stroke:#5FB483
        Buyer["fa:fa-user Buyer"]
        style Buyer fill:#F5DEB3,stroke:#D2B48C
        Seller["fa:fa-user-times Seller"]
        style Seller fill:#ADD8E6,stroke:#4682B4
    end
    
    subgraph Trigger["fa:fa-bolt Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        MintCall["fa:fa-arrow-circle-up Mint Call<br/>(Creator)"]
        style MintCall fill:#E6F2FF,stroke:#B8D8F2
        TransferCall["fa:fa-exchange-alt Transfer Call<br/>(Owner)"]
        style TransferCall fill:#E6F2FF,stroke:#B8D8F2
        BurnCall["fa:fa-trash Burn Call<br/>(Owner)"]
        style BurnCall fill:#E6F2FF,stroke:#B8D8F2
    end

    Creator -- "fa:fa-hand-point-right Interacts With" --> NFTSmartContract
    Buyer -- "fa:fa-hand-point-right Interacts With" --> NFTSmartContract
    Seller -- "fa:fa-hand-point-right Interacts With" --> NFTSmartContract

    NFTSmartContract -- "fa:fa-link Deployed On" --> BlockchainNetwork
    ContractCode -- "fa:fa-pencil-alt Defines" --> StateVariables
    ContractCode -- "fa:fa-pencil-alt Defines" --> Functions
    ContractCode -- "fa:fa-pencil-alt Defines" --> Events
    Functions -- "fa:fa-sync-alt Updates" --> StateVariables
    Events -- "fa:fa-bell Emitted When State Changes" --> BlockchainNetwork

    MintCall --> Functions
    TransferCall --> Functions
    BurnCall --> Functions


```

**An NFT (Non-Fungible Token)** is a unique digital asset represented by a smart contract. The smart contract contains code that defines state variables like token metadata and ownership, and functions for minting, transferring, and burning NFTs. Creators can mint new NFTs, owners can transfer them, and events like transfers and approvals are logged on the blockchain. Participants such as creators, buyers, and sellers interact with the NFT smart contract to manage the lifecycle of NFTs, ensuring transparency and security in ownership.
##  Lending and Borrowing

```mermaid
graph LR
    subgraph DeFiSmartContract["fa:fa-file-contract DeFi Lending/Borrowing Smart Contract"]
        style DeFiSmartContract fill:#FFF2CC,stroke:#D6B656
        ContractCode["fa:fa-code Contract Code<br/>(Solidity, etc.)"]
        StateVariables["fa:fa-database State Variables<br/>(Lender Balances, Borrower Debts)"]
        Functions["fa:fa-cogs Functions<br/>(Deposit, Withdraw, Borrow, Repay)"]
        Events["fa:fa-bullhorn Events<br/>(Deposit, Withdrawal, Borrow, Repay)"]
    end

    subgraph EthereumBlockchain["fa:fa-link Ethereum Blockchain"]
        style EthereumBlockchain fill:#F2F2F2,stroke:#666666
        BlockchainNetwork["fa:fa-network-wired Ethereum Network"]
    end

    subgraph Participants["fa:fa-users Participants"]
        style Participants fill:#F0F0F0,stroke:#888888
        Lender["fa:fa-user-tie Lender"]
        style Lender fill:#C2E0C6,stroke:#5FB483
        Borrower["fa:fa-user Borrower"]
        style Borrower fill:#F5DEB3,stroke:#D2B48C
    end
    
    subgraph Trigger["fa:fa-bolt Triggers"]
        style Trigger fill:#D4EDDA,stroke:#4CAF50
        DepositCall["fa:fa-arrow-circle-down Deposit Call<br/>(Lender)"]
        style DepositCall fill:#E6F2FF,stroke:#B8D8F2
        WithdrawCall["fa:fa-arrow-circle-up Withdraw Call<br/>(Lender)"]
        style WithdrawCall fill:#E6F2FF,stroke:#B8D8F2
        BorrowCall["fa:fa-hand-holding-usd Borrow Call<br/>(Borrower)"]
        style BorrowCall fill:#E6F2FF,stroke:#B8D8F2
        RepayCall["fa:fa-handshake Repay Call<br/>(Borrower)"]
        style RepayCall fill:#E6F2FF,stroke:#B8D8F2
    end

    Lender -- "fa:fa-hand-point-right Interacts With" --> DeFiSmartContract
    Borrower -- "fa:fa-hand-point-right Interacts With" --> DeFiSmartContract

    DeFiSmartContract -- "fa:fa-link Deployed On" --> BlockchainNetwork
    ContractCode -- "fa:fa-pencil-alt Defines" --> StateVariables
    ContractCode -- "fa:fa-pencil-alt Defines" --> Functions
    ContractCode -- "fa:fa-pencil-alt Defines" --> Events
    Functions -- "fa:fa-sync-alt Updates" --> StateVariables
    Events -- "fa:fa-bell Emitted When State Changes" --> BlockchainNetwork

    DepositCall --> Functions
    WithdrawCall --> Functions
    BorrowCall --> Functions
    RepayCall --> Functions

```

**Lenders** deposit funds into a smart contract to earn interest. **Borrowers** take out loans by using collateral and paying interest. Smart contracts manage balances, debts, and ensure transparent transactions. Lenders can withdraw their funds, and borrowers can repay loans through specific function calls. This system enhances accessibility and efficiency in financial services without intermediaries.

## Liquidity Pool

```mermaid
graph LR;

subgraph LiquidityPool["fa:fa-water Liquidity Pool"]
    TokenA["fa:fa-coins Token A"]:::skyblue
    TokenB["fa:fa-coins Token B"]:::limegreen
    AMM["fa:fa-cogs Automated\nMarket Maker"]:::gold
end

User1["fa:fa-user User 1\n(Liquidity\nProvider)"]:::coral -->|fa:fa-arrow-down Deposits\nToken A & B| LiquidityPool
User2["fa:fa-user User 2\n(Liquidity\nProvider)"]:::orchid -->|fa:fa-arrow-down Deposits\nToken A & B| LiquidityPool
LiquidityPool -->|fa:fa-exchange-alt Swaps\nToken A <--> B| User3["fa:fa-user User 3\n(Swapper)"]:::hotpink
LiquidityPool -->|fa:fa-coins Trading\nFees| User1 & User2
AMM -->|fa:fa-balance-scale Determines\nPrice & Executes\nTrades| LiquidityPool

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


## Hard Fork vs Soft Fork

```mermaid
graph TD
    subgraph ethereumBlockchain["fa:fa-link Ethereum Blockchain"]
        genesisBlock["fa:fa-cube Genesis Block"] --> |Initial State| block1["Block 1"] 
        block1 --> |Protocol Rules| block2["Block 2"]
        block2 --> |Existing Rules| block3["Block 3"]
    end
    
    subgraph softFork["fa:fa-code-fork Soft Fork (Backward Compatible)"]
        block3 --> |New Rules, Old Nodes Valid| block4Soft["Block 4"]
        block4Soft --> |Continued Compatibility| block5Soft["Block 5"]
        style softFork fill:#90EE90,stroke:#006400
    end

    subgraph hardFork["fa:fa-code-branch Hard Fork (Non-Compatible)"]
        block3 --> |New Chain, New Rules| block4Hard["Block 4'"]
        block4Hard --> |Separate Chain Continues| block5Hard["Block 5'"]
        style hardFork fill:#FFCCCB,stroke:#8B0000
    end
    
    block3 -.-> block4Soft
    block3 -.-> block4Hard
```

- **Soft Fork**:
  - Introduces backward-compatible changes to the protocol rules.
  - Older nodes can still validate new blocks, even if they don't understand the new rules.
  - Both old and new nodes continue on the same chain.

- **Hard Fork**:
  - Introduces non-backward-compatible changes.
  - Older nodes cannot validate new blocks created under the new rules.
  - This results in a permanent split into two separate chains: The original chain with old nodes following the old rules. A new chain with upgraded nodes following the new rules.


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
    transactionsPerSecond["fa:fa-tachometer-alt Transactions Per Second (TPS)"]
    style transactionsPerSecond fill:#F2F2F2,stroke:#666666

    subgraph factorsInfluencingTps["Factors Influencing TPS"]
        style factorsInfluencingTps fill:#D4EDDA,stroke:#4CAF50
        blockSize["fa:fa-expand-arrows-alt Block Size"]
        style blockSize fill:#FFF9E6,stroke:#F9EBC8
        blockTime["fa:fa-clock Block Time"]
        style blockTime fill:#FFF9E6,stroke:#F9EBC8
        consensusMechanism["fa:fa-cogs Consensus Mechanism"]
        style consensusMechanism fill:#FFF9E6,stroke:#F9EBC8
        transactionComplexity["fa:fa-search-dollar Transaction Complexity"]
        style transactionComplexity fill:#FFF9E6,stroke:#F9EBC8
    end

    subgraph benefitsHighTps["Benefits of High TPS"]
        style benefitsHighTps fill:#C2E0C6,stroke:#5FB483
        scalability["fa:fa-chart-line Scalability"]
        lowFees["fa:fa-dollar-sign Low Fees"]
        fastTransactions["fa:fa-bolt Fast Transactions"]
        betterUserExperience["fa:fa-thumbs-up Better User Experience"]
    end

    subgraph drawbacksHighTps["Drawbacks of High TPS"]
        style drawbacksHighTps fill:#F2DEDE,stroke:#D9534F
        centralizationRisk["fa:fa-map-pin Centralization Risk"]
        potentialSecurityRisks["fa:fa-shield-alt Potential Security Risks"]
        higherCosts["fa:fa-money-bill-wave Higher Costs"]
        ddosAttackRisk["fa:fa-shield-virus DDoS Attack Risk"]
    end

    subgraph examplesTpsBlockchains["Examples of TPS in Blockchains"]
        style examplesTpsBlockchains fill:#E1D5E7,stroke:#9673A6
        bitcoin["fa:fa-b Bitcoin (BTC) ~4-7 TPS"]
        ethereum["fa:fa-e Ethereum (ETH) ~15-45 TPS"]
        solana["fa:fa-s Solana (SOL) ~50,000 TPS (theoretical)"]
    end

    transactionsPerSecond --> |"Benefits"|benefitsHighTps
    transactionsPerSecond --> |"Drawbacks"|drawbacksHighTps
    blockSize --> |"Influences"|transactionsPerSecond
    blockTime --> |"Influences"|transactionsPerSecond
    consensusMechanism --> |"Influences"|transactionsPerSecond
    transactionComplexity --> |"Influences"|transactionsPerSecond
    transactionsPerSecond --> |"Examples"|examplesTpsBlockchains

```
**TPS**: measures a blockchain's transaction processing speed, influenced by block size, time, consensus, and complexity.
High TPS offers scalability and fast transactions, but can risk security and decentralization.

## Layer 0, 1, 2

```mermaid
graph 
    subgraph Layer0["fa:fa-network-wired Layer 0"]
        style Layer0 fill:#F0E68C,stroke:#BDB76B
        Layer0Network1["fa:fa-cube Network 1"]
        Layer0Network2["fa:fa-cube Network 2"]
        Layer0Network1 --> Layer0Network2
    end

    subgraph ParentChain["fa:fa-sitemap Parent Chain (L1)"]
        style ParentChain fill:#DCDCDC,stroke:#696969
        ParentBlock1["fa:fa-cube Block 1"]
        ParentBlock2["fa:fa-cube Block 2"]
        ParentBlock3["fa:fa-cube Block 3"]
        ParentBlock1 --> ParentBlock2 --> ParentBlock3
    end

    subgraph ChildChain["fa:fa-link Child Chain (L2)"]
        style ChildChain fill:#95E1D3,stroke:#00868B
        ChildBlock1["fa:fa-cube Block 1"]
        ChildBlock2["fa:fa-cube Block 2"]
        ChildBlock3["fa:fa-cube Block 3"]
        ChildBlock1 --> ChildBlock2 --> ChildBlock3
    end

    Layer0Network1 --> |"Infrastructure Support"| ParentChain
    Layer0Network2 --> |"Infrastructure Support"| ParentChain
    ChildBlock3 --> |"Transaction Data"| Checkpoint["fa:fa-flag-checkered Checkpoint"]
    Checkpoint --> |"State Root"| ParentBlock2

```
- **Layer 0 (L0)**: The foundation layer of blockchain ecosystems, providing the underlying infrastructure and protocols that enable interoperability between different blockchain networks. L0 solutions aim to solve scalability and communication issues across multiple chains. Examples include Polkadot, Cosmos, and Avalanche.
- **Layer 1 (L1) - Parent Chain**: The base blockchain protocol, such as Bitcoin or Ethereum. This is the main blockchain that provides the fundamental security and consensus mechanisms. L1 chains are typically slower but offer the highest level of security and decentralization. They handle the final settlement of transactions and maintain the network's core functionality.
- **Layer 2 (L2) - Child Chain**: Scaling solutions built on top of Layer 1 to improve transaction speed and reduce costs. L2 solutions process most transactions off the main chain and periodically settle them on the L1 network. Examples include Lightning Network for Bitcoin and Optimistic Rollups for Ethereum. L2 solutions offer faster and cheaper transactions while leveraging the security of the underlying L1 network.


## Sharding

```mermaid
graph LR
    subgraph beaconChain["fa:fa-sitemap Beacon Chain"]
        style beaconChain fill:#F2F2F2,stroke:#666666
        beaconNode1["fa:fa-server Beacon Node 1"]
        beaconNode2["fa:fa-server Beacon Node 2"]
        beaconNode3["fa:fa-server Beacon Node 3"]
        beaconNode1 --> |"Connects to"|beaconNode2
        beaconNode2 --> |"Connects to"|beaconNode3
    end

    subgraph shardChains["fa:fa-network-wired Shard Chains"]
        style shardChains fill:#C2E0C6,stroke:#5FB483
        shardChain1["fa:fa-link Shard Chain 1"]
        shardChain2["fa:fa-link Shard Chain 2"]
        shardChain3["fa:fa-link Shard Chain 3"]
    end

    beaconChain --> |"Links to"|shardChain1
    beaconChain --> |"Links to"|shardChain2
    beaconChain --> |"Links to"|shardChain3

    subgraph shardChain1["Shard Chain 1"]
        style shardChain1 fill:#E6F2FF,stroke:#B8D8F2
        shardBlock1["fa:fa-cube Shard Block 1"]
        shardBlock2["fa:fa-cube Shard Block 2"]
        shardBlock1 --> |"Connects to"|shardBlock2
    end

    subgraph shardChain2["Shard Chain 2"]
        style shardChain2 fill:#E6F2FF,stroke:#B8D8F2
        shardBlock3["fa:fa-cube Shard Block 3"]
        shardBlock4["fa:fa-cube Shard Block 4"]
        shardBlock3 --> |"Connects to"|shardBlock4
    end

    subgraph shardChain3["Shard Chain 3"]
        style shardChain3 fill:#E6F2FF,stroke:#B8D8F2
        shardBlock5["fa:fa-cube Shard Block 5"]
        shardBlock6["fa:fa-cube Shard Block 6"]
        shardBlock5 --> |"Connects to"|shardBlock6
    end

```

- **Beacon Chain** (Coordinator): This is the main chain that manages the entire sharded network. It assigns validators to different shard chains and ensures consensus across the network.

- **Shard Chains** (Parallel Processing): These are smaller chains that run in parallel, each processing a portion of the network's transactions and state. This parallel processing significantly increases the overall transaction throughput of the blockchain.

- **Shard Blocks**: Blocks on individual shard chains contain transactions and data relevant to that specific shard.

## Blockchain Trilelema (DSS)

```mermaid
graph LR
    subgraph BlockchainTrilemma["Blockchain Trilemma"]
        Decentralization["fa:fa-network-wired Decentralization"]
        Scalability["fa:fa-expand Scalability"]
        Security["fa:fa-lock Security"]
    end

  
    subgraph Explanation["Explanation"]
        ScalabilityDef["Ability to handle\nmany transactions"]
        SecurityDef["Protection against\nattacks and failures"]
        DecentralizationDef["Distribution of\ncontrol and power"]
    end

    Scalability --- ScalabilityDef
    Security --- SecurityDef
    Decentralization --- DecentralizationDef

    classDef primary fill:#FFFFDE,stroke:#333,stroke-width:2px;
    classDef secondary fill:#DEFFEF,stroke:#333,stroke-width:2px;
    classDef tertiary fill:#DEDEFF,stroke:#333,stroke-width:2px;

    class BlockchainTrilemma primary;
    class Explanation secondary;
    class Scalability,Security,Decentralization tertiary;

```


The **blockchain trilemma** is a concept coined by **Vitalik Buterin** that proposes a set of three main issues — decentralization, security and scalability — that developers encounter when building blockchains, forcing them to ultimately sacrifice one "aspect" for as a trade-off to accommodate the other two. 


## Spot Trading

Spot trading is the buying and selling of assets for immediate delivery and payment. It involves the exchange of assets at the current market price, with transactions settled within a short period. Spot trading is commonly used in traditional financial markets and cryptocurrency exchanges.

```mermaid
graph TD
    userHasBTC["📈 User has 1 BTC"]
    buyBTC["📈 Buys 1 BTC at $10,000"]
    sellBTC["📈 Sells 1 BTC at $11,000"]
    profit["💵 Profit: $1,000"]
    loss["🔻 Loss: $1,000"]

    userHasBTC --> |"buys BTC"|buyBTC
    buyBTC --> |"BTC at $11,000"|sellBTC
    sellBTC --> |"profit"|profit
    sellBTC --> |"loss"|loss
```


## Future Trading

Future trading is a financial contract where parties agree to buy or sell an asset at a future date for a predetermined price. It allows investors to speculate on the price movement of an asset without owning it. Future trading is commonly used in commodities, currencies, and cryptocurrencies.

```mermaid
graph TD
    userHasBTC["📈 User has 1 BTC"]
    openLongFutures["📄 Opens long futures position with 10x leverage"]
    priceOutcome["📅 After 1 month"]
    profit["💵 Profit: $10,000 if BTC at $11,000"]
    loss["🔻 Loss: $10,000 if BTC at $9,000"]

    userHasBTC --> |"opens position"|openLongFutures
    openLongFutures --> |"1 month duration"|priceOutcome
    priceOutcome --> |"BTC at $11,000"|profit
    priceOutcome --> |"BTC at $9,000"|loss
```


## Short Selling

Short selling is a trading strategy where an investor borrows an asset and sells it at the current price, with the expectation that the price will fall. The investor then buys back the asset at a lower price, returns it to the lender, and profits from the price difference.

```mermaid
graph TD
    userHasBTC["📈 User has 1 BTC"]
    openShortPosition["📄 Opens short position with 10x leverage"]
    priceOutcome["📅 After shorting BTC"]
    profit["💵 Profit: $10,000 if BTC at $9,000"]
    loss["🔻 Loss: $10,000 if BTC at $11,000"]

    userHasBTC --> |"opens short"|openShortPosition
    openShortPosition --> |"price change"|priceOutcome
    priceOutcome --> |"BTC at $9,000"|profit
    priceOutcome --> |"BTC at $11,000"|loss
```



