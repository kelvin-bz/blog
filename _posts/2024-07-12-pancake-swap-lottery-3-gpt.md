Absolutely! Let's recreate the progression of Mermaid diagrams for the PancakeSwap Lottery contract, this time using headers and improved styling:

## Core Contract Structure

```mermaid
graph LR
subgraph PancakeSwapLotteryContract["PancakeSwap Lottery Contract"]
    PancakeSwapLottery --> IERC20 
    PancakeSwapLottery --> IRandomNumberGenerator
    PancakeSwapLottery --> Ownable
    PancakeSwapLottery --> ReentrancyGuard
end
```
This diagram visually represents the `PancakeSwapLottery` contract inheriting from the `Ownable` and `ReentrancyGuard` contracts and using the `IERC20` and `IRandomNumberGenerator` interfaces.


## State Variables and Mappings

```mermaid
graph LR
subgraph PancakeSwapLotteryContract["PancakeSwap Lottery Contract"]
    subgraph Variables["State Variables"]
        currentLotteryId["currentLotteryId: uint256"]
        currentTicketId["currentTicketId: uint256"]
        maxNumberTicketsPerBuyOrClaim["maxNumberTicketsPerBuyOrClaim: uint256"]
        maxPriceTicketInCake["maxPriceTicketInCake: uint256"]
        minPriceTicketInCake["minPriceTicketInCake: uint256"]
        pendingInjectionNextLottery["pendingInjectionNextLottery: uint256"]
        cakeToken["cakeToken: IERC20"]
        randomGenerator["randomGenerator: IRandomNumberGenerator"]
    end
    subgraph Mappings["_ Mappings"]
        _lotteries["_lotteries: mapping(uint256 => Lottery)"]
        _tickets["_tickets: mapping(uint256 => Ticket)"]
        _bracketCalculator["_bracketCalculator: mapping(uint32 => uint32)"]
        _numberTicketsPerLotteryId["_numberTicketsPerLotteryId: mapping(uint256 => mapping(uint32 => uint256))"]
        _userTicketIdsPerLotteryId["_userTicketIdsPerLotteryId: mapping(address => mapping(uint256 => uint256[]))"]
    end
end
```
The above diagram now shows the key state variables (e.g., `currentLotteryId`, `cakeToken`) and the mappings (e.g., `_lotteries`, `_tickets`) that store the lottery data and user information.

## Function Overview

```mermaid
graph 
subgraph PancakeSwapLotteryContract["PancakeSwap Lottery Contract"]
    buyTickets["buyTickets(uint256, uint32[])"]
    claimTickets["claimTickets(uint256, uint256[], uint32[])"]
    closeLottery["closeLottery(uint256)"]
    drawFinalNumberAndMakeLotteryClaimable["drawFinalNumberAndMakeLotteryClaimable(uint256, bool)"]
    injectFunds["injectFunds(uint256, uint256)"]
    startLottery["startLottery(...)"]
end
```

This updated diagram displays the main functions of the `PancakeSwapLottery` contract, providing a clear overview of the contract's capabilities.

## Function Flow: `buyTickets`

```mermaid
flowchart TD
    subgraph buyTickets["buyTickets(uint256, uint32[])"]
        A[Start] --> B{Lottery Open & Time Valid?}
        B -- Yes --> C[Calculate Total Cost]
        C --> D[Transfer CAKE Tokens]
        D --> E[Update Lottery Data]
        E --> F[Store Ticket Info]
        F --> G[Increment Ticket ID]
        G --> H[Emit TicketsPurchase Event]
        H --> I[End]
        B -- No --> I[End]
    end

    style buyTickets fill:#e3f2fd,stroke:#00b0ff
```
This flowchart details the `buyTickets` function's execution flow, from checking if the lottery is open to emitting an event upon successful ticket purchase.


## Events

```mermaid
graph 
    subgraph PancakeSwapLotteryContract["PancakeSwap Lottery Contract"]
        AdminTokenRecovery["AdminTokenRecovery(address, uint256)"]
        LotteryClose["LotteryClose(uint256, uint256)"]
        LotteryInjection["LotteryInjection(uint256, uint256)"]
        LotteryOpen["LotteryOpen(uint256, uint256, uint256, uint256, uint256, uint256)"]
        LotteryNumberDrawn["LotteryNumberDrawn(uint256, uint256, uint256)"]
        NewOperatorAndTreasuryAndInjectorAddresses["NewOperatorAndTreasuryAndInjectorAddresses(address, address, address)"]
        NewRandomGenerator["NewRandomGenerator(address)"]
        TicketsPurchase["TicketsPurchase(address, uint256, uint256)"]
        TicketsClaim["TicketsClaim(address, uint256, uint256, uint256)"]
    end
style PancakeSwapLotteryContract fill:#e1f5fe,stroke:#81d4fa
```


## Claim Tickets Function
This diagram details the steps involved in the `claimTickets` function.

```mermaid
graph 
    subgraph claimTicketsFunction["fa:fa-hand-holding-usd Claim Tickets Function"]
    checkClaimConditions["fa:fa-check Check Claim Conditions"]
    calculateRewards["fa:fa-calculator Calculate Rewards for Each Ticket"]
    transferRewards["fa:fa-exchange-alt Transfer CAKE Rewards to User"]
    updateTicketStatus["fa:fa-sync-alt Update Ticket Ownership"]
    emitTicketsClaimed["fa:fa-bullhorn Emit TicketsClaim Event"]
    end

    style claimTicketsFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkClaimConditions fill:#ccffeb,stroke:#333,stroke-width:2px
    style calculateRewards fill:#ebccff,stroke:#333,stroke-width:2px
    style transferRewards fill:#ffccff,stroke:#333,stroke-width:2px
    style updateTicketStatus fill:#ccf2ff,stroke:#333,stroke-width:2px
    style emitTicketsClaimed fill:#f2ccff,stroke:#333,stroke-width:2px
```

## Close Lottery Function
This diagram details the steps involved in the `closeLottery` function.

```mermaid
graph 
    subgraph closeLotteryFunction["fa:fa-stop Close Lottery Function"]
    checkLotteryStatus["fa:fa-check Check Lottery Status"]
    requestRandomNumber["fa:fa-dice Request Random Number"]
    updateLotteryStatus["fa:fa-sync-alt Update Lottery Status to Close"]
    emitLotteryClosed["fa:fa-bullhorn Emit LotteryClose Event"]
    end

    style closeLotteryFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkLotteryStatus fill:#ccffeb,stroke:#333,stroke-width:2px
    style requestRandomNumber fill:#ebccff,stroke:#333,stroke-width:2px
    style updateLotteryStatus fill:#ffccff,stroke:#333,stroke-width:2px
    style emitLotteryClosed fill:#ccf2ff,stroke:#333,stroke-width:2px
```

## Draw Final Number Function
This diagram details the steps involved in the `drawFinalNumberAndMakeLotteryClaimable` function.

```mermaid
graph 
    subgraph drawFinalNumberFunction["fa:fa-draw-polygon Draw Final Number"]
    checkCloseStatus["fa:fa-check Check Lottery Close Status"]
    getRandomResult["fa:fa-dice Get Random Result from RNG"]
    calculateRewardsDistribution["fa:fa-calculator Calculate Rewards Distribution"]
    updateStatusToClaimable["fa:fa-sync-alt Update Lottery Status to Claimable"]
    transferToTreasury["fa:fa-piggy-bank Transfer Remaining CAKE to Treasury"]
    emitFinalNumberDrawn["fa:fa-bullhorn Emit LotteryNumberDrawn Event"]
    end

    style drawFinalNumberFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkCloseStatus fill:#ccffeb,stroke:#333,stroke-width:2px
    style getRandomResult fill:#ebccff,stroke:#333,stroke-width:2px
    style calculateRewardsDistribution fill:#ffccff,stroke:#333,stroke-width:2px
    style updateStatusToClaimable fill:#ccf2ff,stroke:#333,stroke-width:2px
    style transferToTreasury fill:#f2ccff,stroke:#333,stroke-width:2px
    style emitFinalNumberDrawn fill:#ffe6cc,stroke:#333,stroke-width:2px
```

## Start Lottery Function
This diagram details the steps involved in the `startLottery` function.

```mermaid
graph 
    subgraph startLotteryFunction["fa:fa-play Start Lottery Function"]
    checkPreviousLotteryStatus["fa:fa-check Check Previous Lottery Status"]
    validateParameters["fa:fa-check Validate Parameters (Time, Price, Discounts)"]
    initializeLottery["fa:fa-sliders-h Initialize Lottery Details"]
    emitLotteryStarted["fa:fa-bullhorn Emit LotteryOpen Event"]
    end

    style startLotteryFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkPreviousLotteryStatus fill:#ccffeb,stroke:#333,stroke-width:2px
    style validateParameters fill:#ebccff,stroke:#333,stroke-width:2px
    style initializeLottery fill:#ffccff,stroke:#333,stroke-width:2px
    style emitLotteryStarted fill:#ccf2ff,stroke:#333,stroke-width:2px
```

## Inject Funds Function
This diagram details the steps involved in the `injectFunds` function.

```mermaid
graph 
    subgraph injectFundsFunction["fa:fa-money-bill-wave Inject Funds Function"]
    checkLotteryOpen["fa:fa-check Check Lottery Status is Open"]
    transferFundsToContract["fa:fa-exchange-alt Transfer CAKE Tokens to Contract"]
    updateAmountCollected["fa:fa-sync-alt Update Amount Collected"]
    emitFundsInjected["fa:fa-bullhorn Emit LotteryInjection Event"]
    end

    style injectFundsFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkLotteryOpen fill:#ccffeb,stroke:#333,stroke-width:2px
    style transferFundsToContract fill:#ebccff,stroke:#333,stroke-width:2px
    style updateAmountCollected fill:#ffccff,stroke:#333,stroke-width:2px
    style emitFundsInjected fill:#ccf2ff,stroke:#333,stroke-width:2px
```

