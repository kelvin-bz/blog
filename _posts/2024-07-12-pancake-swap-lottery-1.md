---
title: "Learn from Pancakeswap lottery smart contract"
categories: [tech]
date: 2024-07-12 00:00:00
tags: [solidity, smart-contracts]
image: "/assets/images/lottery-smart-contract/thumbnail.png"
---

### Overview of the PancakeSwap Lottery Contract

#### Introduction to PancakeSwap Lottery

The PancakeSwap lottery contract is a decentralized application (dApp) that allows users to participate in a lottery system using the Binance Smart Chain (BSC). The contract is designed to be transparent, secure, and fair, providing users with an opportunity to win rewards based on their ticket purchases.

The contract code [https://bscscan.com/address/0x5af6d33de2ccec94efb1bdf8f92bd58085432d2c#code](https://bscscan.com/address/0x5af6d33de2ccec94efb1bdf8f92bd58085432d2c#code)



#### Basic Structure of the Contract


```mermaid
graph 
    subgraph basicStructure["fa:fa-file-code Basic Contract Structure"]
    contractDeclaration["fa:fa-scroll Contract Declaration"]
    interfacesImports["fa:fa-plug Interfaces and Imports"]
    stateVariables["fa:fa-database State Variables"]
    constructorFunction["fa:fa-hammer Constructor Function"]
    modifierFunctions["fa:fa-shield-alt Modifier Functions"]
    mainFunctions["fa:fa-tasks Main Functions"]
    end

    style basicStructure fill:#,stroke:#333,stroke-width:4px
    style contractDeclaration fill:#ccf,stroke:#333,stroke-width:2px
    style interfacesImports fill:#cfc,stroke:#333,stroke-width:2px
    style stateVariables fill:#ffc,stroke:#333,stroke-width:2px
    style constructorFunction fill:#fcc,stroke:#333,stroke-width:2px
    style modifierFunctions fill:#ccf,stroke:#333,stroke-width:2px
    style mainFunctions fill:#cfc,stroke:#333,stroke-width:2px

```

```mermaid
graph
subgraph contractStructure["fa:fa-file-code Contract Structure"]
    basicStructure["fa:fa-file-code Basic Contract Structure"]
    
    subgraph addresses["fa:fa-address-book Addresses"]
        treasuryAddress["Treasury Address"]
        operatorAddress["Operator Address"]
        injectorAddress["Injector Address"]
    end
    
    subgraph structs["fa:fa-folder-open Structs"]
        lotteryStruct["Lottery"]
        ticketStruct["Ticket"]
    end
    
    subgraph mappings["fa:fa-map Mappings"]
        lotteriesMapping["_lotteries"]
        ticketsMapping["_tickets"]
    end
end

subgraph modifiers["fa:fa-exclamation-triangle Modifiers"]
    onlyRandomGeneratorMod["onlyRandomGenerator"]
    notContractMod["notContract"]
end

subgraph functions["fa:fa-cog Functions"]
    createNewLottoFun["createNewLotto"]
    drawWinningNumbersFun["drawWinningNumbers"]
    batchBuyLottoTicketFun["batchBuyLottoTicket"]
    claimRewardFun["claimReward"]
    batchClaimRewardsFun["batchClaimRewards"]
end

subgraph events["fa:fa-bullhorn Events"]
    NewBatchMintEvent["NewBatchMint"]
    RequestNumbersEvent["RequestNumbers"]
    UpdatedSizeOfLotteryEvent["UpdatedSizeOfLottery"]
    UpdatedMaxRangeEvent["UpdatedMaxRange"]
    UpdatedBucketsEvent["UpdatedBuckets"]
    LotteryOpenEvent["LotteryOpen"]
    LotteryCloseEvent["LotteryClose"]
end


classDef default stroke:#ffab40,fill:#fff2cc,color:#000
classDef special stroke:#00b0ff,fill:#e3f2fd,color:#000
classDef structs stroke:#a5d6a7,fill:#e8f5e9,color:#000
classDef mappings stroke:#90caf9,fill:#e3f2fd,color:#000
classDef functions stroke:#f48fb1,fill:#fce4ec,color:#000
classDef modifiers stroke:#ffe082,fill:#fff3e0,color:#000
classDef events stroke:#81d4fa,fill:#e1f5fe,color:#000

class contractStructure special
class addresses default
class structs structs
class mappings mappings
class functions functions
class modifiers modifiers
class events events

```

```mermaid
graph LR
    keyComponents["fa:fa-puzzle-piece Key Components"]
    keyComponents --> |inherits|context["fa:fa-code Context"]
    keyComponents --> |inherits|ownable["fa:fa-user-shield Ownable"]
    keyComponents --> |inherits|reentrancyGuard["fa:fa-shield-alt Reentrancy Guard"]
    keyComponents --> |uses|safeERC20["fa:fa-exchange-alt SafeERC20"]
    
    style keyComponents fill:#ffebcc,stroke:#333,stroke-width:2px
    style context fill:#ccffeb,stroke:#333,stroke-width:2px
    style ownable fill:#ebccff,stroke:#333,stroke-width:2px
    style reentrancyGuard fill:#ffebcc,stroke:#333,stroke-width:2px
    style safeERC20 fill:#ccecff,stroke:#333,stroke-width:2px

```

##### ReentrancyGuard

Reentrancy is a type of attack where a malicious contract can repeatedly call a function within your contract before that function has finished executing. This can disrupt the expected flow of your contract's logic and potentially allow attackers to drain funds or manipulate data.

```js
abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}
```

The ReentrancyGuard contract offers a simple yet effective mechanism to prevent reentrancy attacks. It works like a lock that ensures only one function call can be active at a time.

   **`nonReentrant` Modifier:**
   - This is the core of the protection mechanism. It's a modifier that you can attach to functions that you want to safeguard against reentrancy.
   - **Before entering the function:**
      - It checks if `_status` is NOT `_ENTERED`. This means no other function is running, so it's safe to proceed.
      - It sets `_status` to `_ENTERED` to lock the contract, preventing re-entry.
   - **After the function's code (`_;`) executes:**
      - It resets `_status` to `_NOT_ENTERED`, unlocking the contract for further calls.

**Code Wrapping**: When you attach a modifier to a function in Solidity, the compiler doesn't simply call the modifier function before or after the main function. Instead, it wraps the code of your function within the modifier's code.

Similar concept in javascript

```js
function nonReentrant(originalFunction) {
  return async function (...args) {
    if (this._status === 'ENTERED') {
      throw new Error('Reentrancy detected');
    }
    this._status = 'ENTERED';
    try {
      await originalFunction.apply(this, args);
    } finally {
      this._status = 'NOT_ENTERED';
    }
  };
}

myProtectedFunction = nonReentrant(myProtectedFunction);

```


##### SafeERC20

```js
library SafeERC20 {
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}
```

### State Variables 


```mermaid
graph LR
    subgraph  [" "]
        subgraph lotteriesDetails["fa:fa-database Lotteries"]
        lotteries["fa:fa-database Mapping(uint256 => Lottery)"]
        end

        subgraph ticketsDetails["fa:fa-ticket Tickets"]
        tickets["fa:fa-ticket Mapping(uint256 => Ticket)"]
        end

        subgraph addressesDetails["fa:fa-address-book Addresses"]
        injectorAddress["fa:fa-user-secret Injector Address"]
        operatorAddress["fa:fa-user-cog Operator Address"]
        treasuryAddress["fa:fa-piggy-bank Treasury Address"]
        end

        subgraph parametersDetails["fa:fa-sliders-h Parameters"]
        currentLotteryId["fa:fa-id-card Current Lottery ID"]
        currentTicketId["fa:fa-ticket-alt Current Ticket ID"]
        maxNumberTicketsPerBuyOrClaim["fa:fa-sort-numeric-up Max Tickets Per Buy/Claim"]
        maxPriceTicketInCake["fa:fa-money-check-alt Max Price Ticket in CAKE"]
        minPriceTicketInCake["fa:fa-money-bill-wave Min Price Ticket in CAKE"]
        pendingInjectionNextLottery["fa:fa-arrow-alt-circle-right Pending Injection Next Lottery"]
        end

        lotteriesDetails
        ticketsDetails
        addressesDetails
        parametersDetails

        style lotteriesDetails fill:#ccffeb,stroke:#333,stroke-width:2px
        style ticketsDetails fill:#ebccff,stroke:#333,stroke-width:2px
        style addressesDetails fill:#ffccff,stroke:#333,stroke-width:2px
        style parametersDetails fill:#ccf2ff,stroke:#333,stroke-width:2px

        style lotteries fill:#ccffeb,stroke:#333,stroke-width:2px
        style tickets fill:#ebccff,stroke:#333,stroke-width:2px
        style injectorAddress fill:#ffccff,stroke:#333,stroke-width:2px
        style operatorAddress fill:#ffccff,stroke:#333,stroke-width:2px
        style treasuryAddress fill:#ffccff,stroke:#333,stroke-width:2px
        style currentLotteryId fill:#ccf2ff,stroke:#333,stroke-width:2px
        style currentTicketId fill:#ccf2ff,stroke:#333,stroke-width:2px
        style maxNumberTicketsPerBuyOrClaim fill:#ccf2ff,stroke:#333,stroke-width:2px
        style maxPriceTicketInCake fill:#ccf2ff,stroke:#333,stroke-width:2px
        style minPriceTicketInCake fill:#ccf2ff,stroke:#333,stroke-width:2px
        style pendingInjectionNextLottery fill:#ccf2ff,stroke:#333,stroke-width:2px
    end
```

###  Modifiers

#### Access Control Modifiers

Access control modifiers are essential in smart contracts to ensure that only authorized users can execute certain functions. In the PancakeSwap lottery contract, there are three main access control modifiers: `onlyOwner`, `onlyOperator`, and `notContract`.

- **`onlyOwner` Modifier**: Restricts access to the owner of the contract.
- **`onlyOperator` Modifier**: Restricts access to the designated operator address.
- **`notContract` Modifier**: Restricts access to non-contract addresses (prevents contracts from interacting with certain functions).

Here's a detailed explanation of each modifier along with a visual representation:

```mermaid
graph TD
    accessModifiers["fa:fa-key Access Control Modifiers"]
    accessModifiers --> |restricts|onlyOwner["fa:fa-user-shield Only Owner"]
    accessModifiers --> |restricts|onlyOperator["fa:fa-user-cog Only Operator"]
    accessModifiers --> |restricts|notContract["fa:fa-ban Not Contract"]

    style accessModifiers fill:#ffebcc,stroke:#333,stroke-width:2px
    style onlyOwner fill:#ccffeb,stroke:#333,stroke-width:2px
    style onlyOperator fill:#ebccff,stroke:#333,stroke-width:2px
    style notContract fill:#ffccff,stroke:#333,stroke-width:2px
```



1. **`onlyOperator` Modifier**
    ```solidity
    modifier onlyOperator() {
        require(msg.sender == operatorAddress, "Not operator");
        _;
    }
    ```
    This modifier restricts access to the operator address. The operator is a designated address that has permission to perform specific operational functions within the contract.

2. **`notContract` Modifier**
    ```solidity
    modifier notContract() {
        require(!_isContract(msg.sender), "Contract not allowed");
        require(msg.sender == tx.origin, "Proxy contract not allowed");
        _;
    }

    function _isContract(address _addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(_addr)
        }
        return size > 0;
    }
    ```
    This modifier ensures that only externally owned accounts (EOAs) can call the function, preventing other contracts from interacting with it. This is useful for preventing automated interactions and ensuring that functions are called by real users.

#### Who is the Operator?

The operator is an address with specific permissions to manage certain aspects of the lottery contract. This address is typically set by the owner and can perform functions like starting and closing lotteries, injecting funds, and drawing numbers.

- **Code Snippet for Setting the Operator:**
    ```solidity
    address public operatorAddress;

    function setOperator(address _operatorAddress) external onlyOwner {
        require(_operatorAddress != address(0), "Cannot be zero address");
        operatorAddress = _operatorAddress;
    }
    ```

- **Functions Restricted to the Operator:**
    - `startLottery`
    - `closeLottery`
    - `drawFinalNumberAndMakeLotteryClaimable`

These access control mechanisms are crucial for maintaining the integrity and security of the PancakeSwap lottery contract, ensuring that only authorized parties can perform critical operations.

Absolutely! Here are the updated Mermaid diagrams, removing the root nodes and using subgraphs instead, as per your request:

### Core Functions

#### Buying Tickets

```mermaid
graph
    subgraph buyTicketsFunction["Buy Tickets Function"]
        checkTicketNumbers["fa:fa-check Check Ticket Numbers (Range, Duplicates)"]
        checkLotteryOpen["fa:fa-check Check Lottery Status & Time"]
        calculatePrice["fa:fa-calculator Calculate Total Price (with Discount)"]
        transferTokens["fa:fa-exchange-alt Transfer CAKE Tokens to Contract"]
        updateLotteryState["fa:fa-sync-alt Update Amount Collected, Ticket IDs, Number Counts"]
        emitTicketsPurchased["fa:fa-bullhorn Emit TicketsPurchase Event"]
    end
```

#### Claiming Tickets

```mermaid
graph
    subgraph claimTicketsFunction["Claim Tickets Function"]
        checkClaimValidity["fa:fa-check Check Ticket IDs, Brackets, Ownership"]
        checkLotteryClaimable["fa:fa-check Check Lottery Status"]
        calculateRewards["fa:fa-calculator Calculate Rewards per Ticket & Bracket"]
        transferRewards["fa:fa-exchange-alt Transfer Total CAKE Rewards to User"]
        updateTicketOwners["fa:fa-sync-alt Update Ticket Ownership to Zero Address"]
        emitTicketsClaimed["fa:fa-bullhorn Emit TicketsClaim Event"]
    end
```


#### Starting a Lottery

```mermaid
graph 
    subgraph startLotteryFunction["Start Lottery Function"]
        validateParameters["fa:fa-check Validate End Time, Prices, Rewards Breakdown, Treasury Fee"]
        incrementLotteryId["fa:fa-plus-circle Increment currentLotteryId"]
        initializeLottery["fa:fa-database Create New Lottery Struct with Parameters"]
        resetPendingInjection["fa:fa-eraser Set pendingInjectionNextLottery to 0"]
        emitLotteryOpen["fa:fa-bullhorn Emit LotteryOpen Event"]
    end
```

#### Closing a Lottery

```mermaid
graph
    subgraph closeLotteryFunction["Close Lottery Function"]
        checkLotteryOpen["fa:fa-check Check Lottery Status & Time"]
        updateNextLotteryTicketId["fa:fa-arrow-right Set firstTicketIdNextLottery"]
        requestRandomNumber["fa:fa-dice Request Random Number from Generator"]
        updateLotteryStatus["fa:fa-sync-alt Set Lottery Status to Close"]
        emitLotteryClose["fa:fa-bullhorn Emit LotteryClose Event"]
    end
```


#### Random Number Generation

```mermaid
graph
    subgraph randomNumberFunction["Random Number Generation (External Contract)"]
        requestRandomness["fa:fa-magic Request Randomness using Seed"]
        receiveRandomness["fa:fa-check-circle Receive & Store Random Number"] 
        updateState["fa:fa-sync-alt Update Lottery State if Applicable"] 
    end
```

#### Drawing the Winning Number

```mermaid
graph
    subgraph drawNumberFunction["Draw Final Number"]
        checkLotteryClosed["fa:fa-check Check Lottery Status"]
        getRandomNumber["fa:fa-dice Get Random Number from Generator"]
        calculateWinners["fa:fa-users Calculate Winners per Bracket"]
        distributePrizes["fa:fa-money-bill-wave Distribute CAKE to Winners & Treasury"]
        updateLotteryState["fa:fa-sync-alt Update Lottery Status, Final Number, etc."]
        emitNumberDrawn["fa:fa-bullhorn Emit LotteryNumberDrawn Event"]
    end
```

