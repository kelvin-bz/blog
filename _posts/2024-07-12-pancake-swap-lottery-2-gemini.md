
## Basic Contract Structure
This diagram gives a high-level overview of the PancakeSwap Lottery contract's structure, including its key components.

```mermaid
graph
    subgraph basicStructure["fa:fa-file-code Basic Contract Structure"]
    contractDeclaration["fa:fa-scroll Contract Declaration"]
    interfacesImports["fa:fa-plug Interfaces and Imports"]
    stateVariables["fa:fa-database State Variables"]
    constructorFunction["fa:fa-cogs Constructor Function"]
    modifierFunctions["fa:fa-shield-alt Modifier Functions"]
    mainFunctions["fa:fa-tasks Main Functions"]
    end

    style basicStructure fill:#ffebcc,stroke:#333,stroke-width:2px
    style contractDeclaration fill:#ccffeb,stroke:#333,stroke-width:2px
    style interfacesImports fill:#ebccff,stroke:#333,stroke-width:2px
    style stateVariables fill:#ffc,stroke:#333,stroke-width:2px
    style constructorFunction fill:#fcc,stroke:#333,stroke-width:2px
    style modifierFunctions fill:#ccf,stroke:#333,stroke-width:2px
    style mainFunctions fill:#cfc,stroke:#333,stroke-width:2px
```

## Key Components
This diagram shows the key components inherited and used by the PancakeSwap Lottery contract.

```mermaid
graph LR
    keyComponents["fa:fa-puzzle-piece Key Components"]
    keyComponents --> |inherits|context["fa:fa-code Context"]
    keyComponents --> |inherits|ownable["fa:fa-user-shield Ownable"]
    keyComponents --> |inherits|reentrancyGuard["fa:fa-shield-alt Reentrancy Guard"]
    keyComponents --> |uses|safeERC20["fa:fa-exchange-alt SafeERC20"]
    keyComponents --> |uses|IERC20["fa:fa-dollar-sign IERC20"]
    keyComponents --> |uses|randomNumberGenerator["fa:fa-dice Random Number Generator"]
    keyComponents --> |uses|pancakeSwapLotteryInterface["fa:fa-plug PancakeSwap Lottery Interface"]

    style keyComponents fill:#ffebcc,stroke:#333,stroke-width:2px
    style context fill:#ccffeb,stroke:#333,stroke-width:2px
    style ownable fill:#ebccff,stroke:#333,stroke-width:2px
    style reentrancyGuard fill:#ffccff,stroke:#333,stroke-width:2px
    style safeERC20 fill:#ccf2ff,stroke:#333,stroke-width:2px
    style IERC20 fill:#f2ccff,stroke:#333,stroke-width:2px
    style randomNumberGenerator fill:#ffe6cc,stroke:#333,stroke-width:2px
    style pancakeSwapLotteryInterface fill:#e6ccff,stroke:#333,stroke-width:2px
```

## State Variables
This diagram details the state variables used in the contract, organized into relevant categories.

```mermaid
graph 
    stateVariables["fa:fa-database State Variables"]

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

    stateVariables --> |stores|lotteriesDetails
    stateVariables --> |stores|ticketsDetails
    stateVariables --> |stores|addressesDetails
    stateVariables --> |stores|parametersDetails

    style stateVariables fill:#ffebcc,stroke:#333,stroke-width:2px
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
```

## Access Control Modifiers
This diagram shows the access control modifiers and their associated access restrictions.

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

## Buy Tickets Function
This diagram details the steps involved in the `buyTickets` function.

```mermaid
graph 
    subgraph buyTicketsFunction["fa:fa-ticket-alt Buy Tickets Function"]
    checkTicketNumbers["fa:fa-check Check Ticket Numbers (Range, Duplicates)"]
    checkLotteryOpen["fa:fa-check Check Lottery Status & Time"]
    calculatePrice["fa:fa-calculator Calculate Total Price (with Discount)"]
    transferTokens["fa:fa-exchange-alt Transfer CAKE Tokens to Contract"]
    updateLotteryState["fa:fa-sync-alt Update Amount Collected, Ticket IDs, Number Counts"]
    emitTicketsPurchased["fa:fa-bullhorn Emit TicketsPurchase Event"]
    end

    style buyTicketsFunction fill:#ffebcc,stroke:#333,stroke-width:2px
    style checkTicketNumbers fill:#ccffeb,stroke:#333,stroke-width:2px
    style checkLotteryOpen fill:#ebccff,stroke:#333,stroke-width:2px
    style calculatePrice fill:#ffccff,stroke:#333,stroke-width:2px
    style transferTokens fill:#ccf2ff,stroke:#333,stroke-width:2px
    style updateLotteryState fill:#f2ccff,stroke:#333,stroke-width:2px
    style emitTicketsPurchased fill:#ffe6cc,stroke:#333,stroke-width:2px
```

These diagrams provide a comprehensive overview of the PancakeSwap Lottery contract, from its basic structure to detailed functionalities.