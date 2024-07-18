---
title: "Blockchain Basics P3 - Lottery Contract: A Beginner’s Guide with Remix and Solidity"
categories: [tech]
date: 2024-07-08 00:00:00
tags: [solidity, smart-contracts]
image: "/assets/images/smart-contracts/lottery_smart_contract.jpg"
---

In this guide, we will explore the creation of a simple lottery smart contract using Solidity and Remix. We will cover the key concepts of smart contracts, such as state variables, functions, modifiers, and testing. By the end of this guide, we will have a basic understanding of how to create, deploy, and test a smart contract on the Ethereum blockchain.

## Overview of the Lottery Contract

```mermaid
graph TD
%% Styling
linkStyle default stroke:#333,stroke-width:2px;

subgraph lotteryContract["fa:fa-coins Lottery Contract"]
    prizePool["fa:fa-trophy Prize Pool"]:::prize
    playersList["fa:fa-list Players List"]:::list
    pickWinnerFunc["fa:fa-gavel Pick Winner Func"]:::winner
end

subgraph players["fa:fa-users Players"]
    player1["fa:fa-user Player 1"]:::player
    player2["fa:fa-user Player 2"]:::player
end

manager["fa:fa-user-tie Contract Owner"]:::player

player1 --> |"fa:fa-arrow-right Sends Ether"| lotteryContract
player2 --> |"fa:fa-arrow-right Sends Ether"| lotteryContract

%% Styling for text inside nodes
style prizePool fill:#ffffff,stroke:#080,stroke-width:2px
style playersList fill:#ffffff,stroke:#080,stroke-width:2px
style player1 fill:#ffe0e0,stroke:#800,stroke-width:2px
style player2 fill:#ffe0e0,stroke:#800,stroke-width:2px
style pickWinnerFunc fill:#ffffe0,stroke:#ff0,stroke-width:2px

classDef prize fill:#ffffff,stroke:#080,stroke-width:2px;
classDef list fill:#ffffff,stroke:#080,stroke-width:2px;
classDef player fill:#ffe0e0,stroke:#800,stroke-width:2px;
classDef winner fill:#ffe0a0,stroke:#800,stroke-width:2px;

```

## Set Owner Of The Contract

The Lottery contract has a state variable called manager, which stores the address of the contract owner. The manager is the person who deploys the contract and has special privileges, such as picking the winner.


```mermaid
graph LR

subgraph EthereumNetwork["fa:fa-cube Ethereum Network"]
    transaction["fa:fa-exchange-alt Transaction"]:::transaction
    msgObject["fa:fa-envelope msg Object"]:::msgObject
    subgraph LotteryContract["fa:fa-coins Lottery Contract"]
    constructor2["fa:fa-play constructor()"]:::constructor
    manager["fa:fa-user-tie manager (address)"]:::manager
end

transaction --> |"fa:fa-arrow-right Contains"| msgObject
msgObject --> |"fa:fa-arrow-right Has Property"| msgSender["fa:fa-user msg.sender"]:::msgSender
msgObject --> |"fa:fa-arrow-right Has Property"| msgValue["fa:fa-coins msg.value"]:::msgValue
msgObject --> |"fa:fa-arrow-right Has Property"| msgData["fa:fa-file-alt msg.data"]:::msgData
msgObject --> |"fa:fa-arrow-right Has Property"| msgGas["fa:fa-gas-pump msg.gas"]:::msgGas
msgObject --> |"fa:fa-arrow-right Triggers Execution of"| constructor2
constructor2 --> |"fa:fa-arrow-right Sets Value of"| manager
end



style EthereumNetwork fill:#e0f0e0,stroke:#080,stroke-width:2px
style LotteryContract fill:#f0f0f0,stroke:#666,stroke-width:2px
style transaction fill:#e0f7fa,stroke:#006064,stroke-width:2px
style msgObject fill:#fff3e0,stroke:#e65100,stroke-width:2px
style msgSender fill:#ffe0e0,stroke:#800,stroke-width:2px
style msgValue fill:#fffde7,stroke:#fbc02d,stroke-width:2px
style msgData fill:#e8f5e9,stroke:#43a047,stroke-width:2px
style msgGas fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px
style constructor2 fill:#e1bee7,stroke:#6a1b9a,stroke-width:2px
style manager fill:#c5cae9,stroke:#283593,stroke-width:2px

```

- `msg` Object: The Ethereum network automatically creates a special msg object for the transaction. This object contains relevant details about the transaction:
msg.sender: The address of the account that initiated the transaction.
- The `msg` object may contain other details like the amount of Ether sent (msg.value), the data sent with the transaction (msg.data), and the remaining gas (msg.gas)

- constructor(): When a new Lottery contract is created, its constructor function is executed.
manager. This state variable within the contract is designed to store the address of the contract's owner (the person who deployed it).

## What Happens When You "Deploy" Again

```mermaid
graph LR

subgraph deployer1Actions["fa:fa-user Deployer 1 Actions"]
  deployContract["fa:fa-rocket Deploy Contract"]:::deploy1
end

subgraph deployer2Actions["fa:fa-user Deployer 2 Actions"]
  deployContractAgain["fa:fa-rocket 'Deploy' Contract Again"]:::deploy2
end

subgraph blockchain["fa:fa-cube Ethereum Blockchain"]
    originalContract["fa:fa-file-contract Original Contract\n(Address 1)"]:::contract
    originalContractOwner["fa:fa-user-tie Owner 1"]:::owner
    newContract["fa:fa-file-contract New Contract\n(Address 2)"]:::contract
    newContractOwner["fa:fa-user-tie Owner 2"]:::owner
end

deployContract --> |"fa:fa-arrow-right Creates"| originalContract
originalContract --> |"fa:fa-arrow-right has owner"| originalContractOwner
deployContractAgain --> |"fa:fa-arrow-right Creates"| newContract
newContract --> |"fa:fa-arrow-right has owner"| newContractOwner

style deployer1Actions fill:#e0e0ff,stroke:#008,stroke-width:2px
style deployer2Actions fill:#ffe0e0,stroke:#800,stroke-width:2px
style blockchain fill:#e0f0e0,stroke:#080,stroke-width:2px
style originalContractOwner fill:#fff,stroke:#f,stroke-width:2px

classDef deploy1 fill:#fff,stroke:#008,stroke-width:2px;
classDef deploy2 fill:#fff,stroke:#800,stroke-width:2px;
classDef contract fill:#fff,stroke:#080,stroke-width:2px;
classDef owner fill:#fff,stroke:#f,stroke-width:2px;

```

- **Deployment is a One-Time Event**:  When you deploy a smart contract to the Ethereum blockchain, it's a one-time action. The contract's bytecode (its compiled code) is permanently stored on the blockchain at a specific address.

- **When You "Deploy" Again**: you are essentially creating a new, separate instance of the contract at a different address on the blockchain. This new instance will have its own independent state, and its manager variable will be set to the address of the person who deployed this new instance. The original contract instance, with its original owner, will still exist and function independently.


## Enter Function

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract Lottery {
    address public manager;
    address[] public players;
    
    
    function Lottery() public {
        manager = msg.sender;
    }

    function enter() public payable {
        require(msg.value > .01 ether);
        players.push(msg.sender);
    }
    
}
```
## Get A Random Number Function
```mermaid
graph LR

nonce["fa:fa-key nonce (uint)"]:::variable
getRandomNumber["fa:fa-random getRandomNumber(uint max)"]:::function

subgraph EthereumBlockchain["Global Variables"]
    blockTimestamp["fa:fa-clock block.timestamp"]:::property
    blockDifficulty["fa:fa-balance-scale block.difficulty"]:::property
    msgSender["fa:fa-user msg.sender"]:::property
end

getRandomNumber --> |"Uses"| blockTimestamp
getRandomNumber --> |"Uses"| blockDifficulty
getRandomNumber --> |"Uses"| msgSender
getRandomNumber --> |"Uses"| nonce
getRandomNumber --> |"Generates Hash"| hash["fa:fa-hashtag keccak256 Hash"]:::hash
hash --> |"Modulo max"| randomNumber["fa:fa-list-ol Random Number (0 to max-1)"]:::result

classDef function fill:#e1bee7,stroke:#6a1b9a,stroke-width:2px;
classDef variable fill:#f0f0f0,stroke:#666,stroke-width:2px;
classDef property fill:#e0e0e0,stroke:#000,stroke-width:2px;
classDef hash fill:#ffd700,stroke:#DAA520,stroke-width:2px;
classDef result fill:#90ee90,stroke:#008000,stroke-width:2px;

```

```js
uint private nonce;

function getRandomNumber(uint max) private returns (uint) {
      nonce++;
      return uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty, msg.sender, nonce))) % max;
  }
```

## Pick A Winner Function

```mermaid
graph TD
  subgraph LotteryContract["fa:fa-coins Lottery Contract"]
    A["fa:fa-trophy pickWinner() Function"] --> B["fa:fa-users Get Number of Players"]
    B --> C["fa:fa-random Generate Random Number"]
    C --> D["fa:fa-user Select Winner"]
    D --> E["fa:fa-gift Transfer Prize"]
    E --> F["fa:fa-refresh Reset Players"]
  end

  style LotteryContract fill:#e0f0e0,stroke:#080
  style A fill:#ffe0e0,stroke:#800
  style F fill:#ccffcc,stroke:#060 

```
```js
//..
uint private nonce;

function getRandomNumber(uint max) private returns (uint) {
      nonce++;
      return uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty, msg.sender, nonce))) % max;
  }

function pickWinner() public {
        uint index = getRandomNumber(players.length);
        address winner = players[index];
        // ...
    }
//..    
```



## Sending Ether to the Winner

```mermaid
graph LR
subgraph LotteryContract["fa:fa-coins Lottery Contract"]
  pickWinnerFunction["fa:fa-gavel pickWinner() Function"]
  winnerIndex["fa:fa-hashtag Winner's Index"]
  playersArray["fa:fa-list-ol players[] Array"]
  winnerAddress["fa:fa-user Winner's Address\n(address type)"]
  payableWinner["fa:fa-money-bill-wave payable(winner)"]
  transferFunction["fa:fa-exchange-alt transfer(address(this).balance)"]
  contractAddress["fa:fa-address-card address(this)"]
  contractBalance["fa:fa-coins this.balance"]
end

pickWinnerFunction --> |"1. Determines" | winnerIndex
playersArray & winnerIndex --> |"2. Retrieves"| winnerAddress
winnerAddress --> |"3. Converts to"| payableWinner
payableWinner --> |"4. Calls"| transferFunction
contractAddress --> |"5. Gets"| contractBalance
contractBalance --> |"6. Transfers"| transferFunction

style LotteryContract fill:#e0f0e0,stroke:#080
style winnerAddress fill:#ffe0e0,stroke:#800
style contractBalance fill:#ffffff,stroke:#080
style contractBalance div.label fill:#ffffff,stroke:#080
style contractBalance div.label span fill:gold

```

```js
// ..
  function pickWinner() public {
        uint256 index = getRandomNumber(players.length);
        address winner = players[index];
        // Transfer the contract balance to the winner
        address payable payableWinner = payable(winner);
        uint256 contractBalance = address(this).balance;
        payableWinner.transfer(contractBalance);
        // Reset the players array for the next round
        players = new address[](0);
    }

//.. 
```

- `payableWinner`: In Solidity, the transfer method can only be called on a payable address. The winner address is of type address, and in order to use the transfer method, it needs to be explicitly converted to address payable. This distinction helps prevent accidental transfers to non-payable addresses, enhancing type safety.

- `transferFunction`: The transfer() function is called to transfer the contract balance to the winner.

## Adding a Modifier

```mermaid
graph TD
%% Styling
linkStyle default stroke:#333,stroke-width:2px;

subgraph lotteryContract["fa:fa-coins Lottery Contract"]
    prizePool["fa:fa-trophy Prize Pool"]:::prize
    playersList["fa:fa-list Players List"]:::list

    subgraph restrictedModifier["fa:fa-lock restricted Modifier"]
       pickWinnerFunc["fa:fa-gavel Pick Winner Func"]:::winner
    end
end

subgraph players["fa:fa-users Players"]
    player1["fa:fa-user Player 1"]:::player
    player2["fa:fa-user Player 2"]:::player

end

manager["fa:fa-user-tie Contract Owner"]:::player --> |fa:fa-check can call|pickWinnerFunc

player1 --> |"fa:fa-arrow-right Sends Ether"| lotteryContract
player2 --> |"fa:fa-arrow-right Sends Ether"| lotteryContract

%% Styling for text inside nodes
style prizePool fill:#ffffff,stroke:#080,stroke-width:2px
style playersList fill:#ffffff,stroke:#080,stroke-width:2px
style player1 fill:#ffe0e0,stroke:#800,stroke-width:2px
style player2 fill:#ffe0e0,stroke:#800,stroke-width:2px
style pickWinnerFunc fill:#ffffe0,stroke:#ff0,stroke-width:2px
style restrictedModifier fill:#ffe0e0,stroke:#ff0,stroke-width:2px

classDef prize fill:#ffffff,stroke:#080,stroke-width:2px;
classDef list fill:#ffffff,stroke:#080,stroke-width:2px;
classDef player fill:#ffe0e0,stroke:#800,stroke-width:2px;
classDef winner fill:#ffe0a0,stroke:#800,stroke-width:2px;

```

```js
  function pickWinner() public restricted {
        uint index = getRandomNumber(players.length);
        address winner = players[index];
        // Transfer the contract balance to the winner
        payable(winner).transfer(address(this).balance);
        // Reset the players array for the next round
        players = new address ;
    }

    modifier restricted() {
        require(msg.sender == manager, "Only the manager can call this function");
        _;
    }
```

## Full Contract Code

```js
// // SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Lottery {
    address public manager;
    address[] public players;
    uint256 private nonce;

    constructor() {
        manager = msg.sender;
        nonce = 0;
    }

    function enter() public payable {
        require(msg.value > .01 ether, "Minimum ether required is .01");
        players.push(msg.sender);
    }

    function getRandomNumber(uint256 max) private returns (uint256) {
        nonce++;
        return
            uint256(
                keccak256(
                    abi.encodePacked(
                        block.timestamp,
                        block.difficulty,
                        msg.sender,
                        nonce
                    )
                )
            ) % max;
    }

    function pickWinner() public {
        uint256 index = getRandomNumber(players.length);
        address winner = players[index];
        // Transfer the contract balance to the winner
        address payable payableWinner = payable(winner);
        uint256 contractBalance = address(this).balance;
        payableWinner.transfer(contractBalance);
        // Reset the players array for the next round
        players = new address[](0);
    }

    modifier restricted() {
        require(
            msg.sender == manager,
            "Only the manager can call this function"
        );
        _;
    }

    function getPlayers() public view returns (address[] memory) {
        return players;
    }
}

```

## Remix

Remix is a powerful online IDE for developing smart contracts on the Ethereum blockchain. It provides a Solidity compiler, a debugger, and various plugins to enhance the development experience. One of the plugins available in Remix is the Solidity Unit Testing plugin, which allows you to write and run tests for your smart contracts directly in the IDE.

To use Remix user interface, you can visit [Remix IDE](https://remix.ethereum.org/)

### Why Test Solidity Contracts?

```mermaid
graph LR
  subgraph WhyTestSolidityContracts["fa:fa-check-circle Why Test Solidity Contracts?"]
    correctness["fa:fa-thumbs-up Ensuring Correctness"]:::reason
    bugs["fa:fa-bug Finding Bugs Early"]:::reason
    confidence["fa:fa-shield-alt Building Confidence"]:::reason
  end



  style WhyTestSolidityContracts fill:#e0f7fa,stroke:#006064,stroke-width:2px;
  style correctness fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px;
  style bugs fill:#fff3e0,stroke:#fb8c00,stroke-width:2px;
  style confidence fill:#f1f8e9,stroke:#43a047,stroke-width:2px;
```

Testing your Solidity smart contracts is a crucial practice for several reasons:

- **Ensuring Correctness**: Smart contracts are immutable once deployed on the blockchain. Rigorous testing helps guarantee that your code behaves as intended and meets all requirements. This is especially critical for contracts dealing with financial assets or sensitive data.

- **Finding Bugs Early**: Testing allows you to uncover errors and vulnerabilities before deploying your contract to the mainnet. This prevents costly mistakes and potential exploits that could result in financial loss or damage to your project's reputation.

- **Building Confidence**: Thorough testing instills confidence in your contract's reliability and security. This confidence is essential for attracting users and investors, as well as ensuring the long-term success of your project.

### Generating Test Files in Remix

![remix]({{ site.baseurl }}/assets/images/remix-lottery.png)


Click on the **Generate** button on the Solidity Unit Testing plugin. This will generate a new test contract on the right side.

Inside the test contract, you will find a function named "before all". This function is called before any of the tests are run.

Inside the "before all" function, you will need to deploy the contract that you want to test. 

```js
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.4.22 <0.9.0;
import "remix_tests.sol"; 
import "remix_accounts.sol";
import "../contracts/4_Lottery.sol";

contract LotteryTestSuite is Lottery {
    address acc0 = TestsAccounts.getAccount(0); // manager by default
    address acc1 = TestsAccounts.getAccount(1);
    address acc2 = TestsAccounts.getAccount(2);
    address acc3 = TestsAccounts.getAccount(3);

    function beforeAll() public {
        // Create an instance of the Lottery contract
        manager = acc0;
    }

    /// #sender: account-1
    /// #value: 20000000000000000
    function testEnterAcc1() public payable {
        Assert.equal(msg.value, 20000000000000000, 'value should be 0.02 Eth');
        enter();
        Assert.equal(players.length, 1, 'players array should have 1 entry');
        Assert.equal(players[0], acc1, 'first player should be account-1');
    }

    /// #sender: account-2
    /// #value: 30000000000000000
    function testEnterAcc2() public payable {
        Assert.equal(msg.value, 30000000000000000, 'value should be 0.03 Eth');
        enter();
        Assert.equal(players.length, 2, 'players array should have 2 entries');
        Assert.equal(players[1], acc2, 'second player should be account-2');
    }

    /// #sender: account-3
    /// #value: 40000000000000000
    function testEnterAcc3() public payable {
        Assert.equal(msg.value, 40000000000000000, 'value should be 0.04 Eth');
        enter();
        Assert.equal(players.length, 3, 'players array should have 3 entries');
        Assert.equal(players[2], acc3, 'third player should be account-3');
    }

    /// #sender: account-0
    function testPickWinner() public {
        uint contractBalance = address(this).balance;
        uint[3] memory initialBalances = [
            acc1.balance,
            acc2.balance,
            acc3.balance
        ];

        pickWinner();

        uint[3] memory finalBalances = [
            acc1.balance,
            acc2.balance,
            acc3.balance
        ];

        bool foundWinner = false;
        for (uint i = 0; i < 3; i++) {
            if (finalBalances[i] > initialBalances[i]) {
                Assert.equal(finalBalances[i], initialBalances[i] + contractBalance, "Winner should receive the contract balance");
                foundWinner = true;
            }
        }

        Assert.equal(foundWinner, true, "There should be one winner with an increased balance");
        Assert.equal(players.length, 0, "Players array should be reset");
    }

}
```



### Run the test

Click on the Solidity Unit Testing plugin again.
Unselect all the tests and select the test for the contract.
Click on the "Run" button.

![remix_test]({{ site.baseurl }}/assets/images/remix-lottery-test.png)


### Deploy And Interact With The Contract

![remix_deploy]({{ site.baseurl }}/assets/images/remix-deploy.png)

- **Deploy the contract**: Click on the "Deploy" button in Remix to deploy the Lottery contract to the Ethereum blockchain. This will create a new instance of the contract with its own address.

- **Interact with the contract**: You can interact with the deployed contract by calling its functions. For example, you can call the enter() function to participate in the lottery by sending a minimum amount of Ether. You can also call the pickWinner() function to select a winner and transfer the prize pool to them.

- **Accouunts**: You can use different accounts in Remix to simulate multiple users interacting with the contract. This allows you to test the contract's functionality from various perspectives and ensure that it behaves as expected for different scenarios.


## References
- Stephen Grider Udemey Course: [Ethereum and Solidity: The Complete Developer's Guide](https://www.udemy.com/course/ethereum-and-solidity-the-complete-developers-guide)


<a href="/posts/truffle-pet-shop-tutorial">Next Post: Blockchain Basics P4 - Truffle Pet-Shop Smart Contract</a> 