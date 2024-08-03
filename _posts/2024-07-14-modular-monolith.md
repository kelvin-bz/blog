---
title: "Microservices, Modular Monoliths, or Distributed Monoliths"
categories: [system design]
date: 2024-07-13 00:00:00
tags: [system design]
image: "/assets/images/microservices.jpeg"
---

The architecture landscape is varied, offering a multitude of options to developers. Among the most popular choices are microservices, modular monoliths, and distributed monoliths. Each architecture has its own strengths and weaknesses, making it crucial to understand the differences between them to make an informed decision.

```mermaid

graph LR
  subgraph  
    microservices["fa:fa-cubes Microservices"]
    modularMonolith["fa:fa-layer-group Modular Monolith"]
    distributedMonolith["fa:fa-network-wired Distributed Monolith"]
   end
```


## Modular Monoliths

Modular monoliths are a hybrid approach that combines the benefits of monolithic architecture with modular design. The application is built as a single unit but organized into distinct modules, allowing for better separation of concerns and maintainability.

```mermaid

graph LR
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> moduleOne["🟦 Module 1"]
  modularMonolith --> moduleTwo["🟦 Module 2"]
  modularMonolith --> moduleThree["🟦 Module 3"]
```

```mermaid
graph LR
    subgraph deployment["Deployment"]
    eCommerceMonolith["fa:fa-box E-commerce Monolith"]
    end

    subgraph dataStore["Data Store"]
    monolithDatabase["fa:fa-database Monolith Database"]
    end

    subgraph modules["Modules"]
    productCatalogModule["fa:fa-folder Product Catalog Module"]
    orderModule["fa:fa-folder Order Module"]
    paymentModule["fa:fa-folder Payment Module"]
    userModule["fa:fa-folder User Module"]
    end

    eCommerceMonolith --> |read/write| monolithDatabase
    productCatalogModule -.-> eCommerceMonolith
    orderModule -.-> eCommerceMonolith
    paymentModule -.-> eCommerceMonolith
    userModule -.-> eCommerceMonolith


    style eCommerceMonolith fill:#add8e6,stroke:#000
    style monolithDatabase fill:#add8e6,stroke:#000
    style productCatalogModule fill:#c7f0f2,stroke:#000
    style orderModule fill:#fff2cc,stroke:#000
    style paymentModule fill:#d9ead3,stroke:#000
    style userModule fill:#f4cccc,stroke:#000
    style deployment fill:#f0f0f0,stroke:#000
    style dataStore fill:#f0f0f0,stroke:#000
    style modules fill:#f0f0f0,stroke:#000
```

- Modules are organized by domain or feature, with clear boundaries and interfaces.
- Shared database for all modules, but with well-defined access patterns.
- Changes to one module can be deployed independently, but the entire application is deployed as a single unit.
- Communication between modules is direct (function calls, shared memory).

## Distributed Monoliths

Distributed monoliths are a variation of monolithic architecture where the application is split into multiple components that communicate over a network. While they offer some benefits of distributed systems, they can still suffer from the drawbacks of monolithic architecture.
```mermaid

graph LR
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> serviceA["fa:fa-server Service A"]
  distributedMonolith --> serviceB["fa:fa-server Service B"]
  serviceA --> serviceB
  serviceB --> serviceA
```


```mermaid
graph LR
    subgraph deployment["Deployment"]
    productCatalogService["fa:fa-box Product Catalog Service"]
    orderService["fa:fa-box Order Service"]
    paymentService["fa:fa-box Payment Service"]
    userService["fa:fa-box User Service"]
    end

    subgraph dataStore["Data Store"]
    sharedDatabase["fa:fa-database Shared Database"]
    end

    productCatalogService -->|read/write product data|sharedDatabase
    orderService -->|read/write order data|sharedDatabase
    paymentService -->|read/write payment data|sharedDatabase
    userService -->|read/write user data|sharedDatabase

    orderService -->|process payment|paymentService
    orderService -->|get user info|userService
    orderService -->|get product info|productCatalogService

    style productCatalogService fill:#c7f0f2,stroke:#000
    style orderService fill:#fff2cc,stroke:#000
    style paymentService fill:#d9ead3,stroke:#000
    style userService fill:#f4cccc,stroke:#000
    style sharedDatabase fill:#d5a6bd,stroke:#000 
    style deployment fill:#dae3f3,stroke:#000
    style dataStore fill:#dae3f3,stroke:#000

```

- They share a single database (e.g., one big database for products, orders, users, and payments).
- They communicate through synchronous calls (e.g., the Order Service directly calls the Payment Service to process a payment).
- Changes to one service often require coordinated deployments of other services.

## Microservices

Microservices are a distributed architecture composed of small, independent services, each responsible for a specific function. These services communicate over a network, enabling flexibility, scalability, and resilience.

```mermaid

graph LR
  microservices["fa:fa-cubes Microservices"] --> serviceOne["fa:fa-server Service 1"]
  microservices --> serviceTwo["fa:fa-server Service 2"]
  microservices --> serviceThree["fa:fa-server Service 3"]
```

```mermaid
graph LR
    subgraph external["External"]
        client["fa:fa-user Client"]
    end
    subgraph deployment["Deployment"]
        apiGateway["fa:fa-door-open API Gateway"]
        productCatalogService["fa:fa-box Product Catalog Service"]
        orderService["fa:fa-box Order Service"]
        paymentService["fa:fa-box Payment Service"]
        userService["fa:fa-box User Service"]
    end

    subgraph dataStore["Data Store"]
        productCatalogDB["fa:fa-database Product Catalog DB"]
        orderDB["fa:fa-database Order DB"]
        paymentDB["fa:fa-database Payment DB"]
        userDB["fa:fa-database User DB"]
    end

    subgraph messageQueue["Message Queue"]
        messageBroker["fa:fa-exchange Message Broker"]
    end
    
    client -->|API requests|apiGateway
    apiGateway -->|route requests|productCatalogService
    apiGateway -->|route requests|orderService
    apiGateway -->|route requests|paymentService
    apiGateway -->|route requests|userService

    productCatalogService -->|read/write|productCatalogDB
    orderService -->|read/write|orderDB
    paymentService -->|read/write|paymentDB
    userService -->|read/write|userDB

    orderService -.->|publish event|messageBroker
    paymentService -.->|consume event|messageBroker
    orderService -.->|get user info|userService
    orderService -.->|get product info|productCatalogService

    style apiGateway fill:#e6b8af,stroke:#000
    style productCatalogService fill:#c7f0f2,stroke:#000
    style orderService fill:#fff2cc,stroke:#000
    style paymentService fill:#d9ead3,stroke:#000
    style userService fill:#f4cccc,stroke:#000
    style productCatalogDB fill:#c7f0f2,stroke:#000
    style orderDB fill:#fff2cc,stroke:#000
    style paymentDB fill:#d9ead3,stroke:#000
    style userDB fill:#f4cccc,stroke:#000
    style messageBroker fill:#c5cae9,stroke:#000
    style deployment fill:#f0f0f0,stroke:#000
    style dataStore fill:#f0f0f0,stroke:#000
    style messageQueue fill:#f0f0f0,stroke:#000
    style client fill:#b7b7b7,stroke:#000
    style external fill:#f0f0f0,stroke:#000

```

- Each service has its own dedicated database (or can share with related services, but with clear boundaries)
- Asynchronous communication (message queues, event-driven) preferred, but can use synchronous when necessary with careful design


## Scalability

Microservices excel in scalability. Individual services can be scaled up or down as demand dictates, without impacting the rest of the system. Modular monoliths, conversely, necessitate scaling the entire application. Distributed monoliths offer some scalability, but interdependencies can limit it.

```mermaid

graph LR
  microservices["fa:fa-cubes Microservices"] --> independentScaling["fa:fa-expand-arrows-alt Independent Scaling"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> wholeAppScaling["fa:fa-expand Whole Application Scaling"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> limitedScaling["fa:fa-expand Limited Scaling"]
```

## Technology Flexibility

Microservices embrace technological diversity. Each service can be built using the technology stack best suited for its specific function. In contrast, modular and distributed monoliths typically rely on a unified tech stack across the entire application.

```mermaid
graph LR
  microservices["fa:fa-cubes Microservices"] --> diverseTech["fa:fa-code Diverse Technologies"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> singleTech["fa:fa-code Single Technology"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> singleTech
```

## Deployment

Microservices enable independent deployment of services, allowing for faster release cycles and reduced risk. Modular monoliths require deploying the entire application, while distributed monoliths face similar challenges due to shared codebases and dependencies.

```mermaid
graph LR
  microservices["fa:fa-cubes Microservices"] --> independentDeployment["fa:fa-rocket Independent Deployment"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> wholeAppDeployment["fa:fa-rocket Whole Application Deployment"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> wholeAppDeployment
```

## Team Autonomy

Microservices are often associated with autonomous teams, where each team is responsible for a specific service. Modular monoliths and distributed monoliths typically involve coordinated teams working on different modules or components.

```mermaid

graph LR
  microservices["fa:fa-cubes Microservices"] --> autonomousTeams["fa:fa-users-cog Autonomous Teams"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> coordinatedTeams["fa:fa-users Coordinated Teams"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> coordinatedTeams
```

## Complexity

Microservices introduce the intricacies of distributed systems—communication overhead, data consistency challenges, and increased operational complexity. Modular monoliths, with their unified codebase, tend to be simpler to manage. Distributed monoliths, while offering the illusion of separation, often grapple with the complexities of microservices without reaping the full benefits.

```mermaid

graph LR
  microservices["fa:fa-cubes Microservices"] --> distributedComplexity["fa:fa-wrench Distributed Complexity"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> monolithicSimplicity["fa:fa-tools Monolithic Simplicity"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> hiddenComplexity["fa:fa-question-circle Hidden Complexity"]
```

## Error Isolation & Resilience

In a microservices architecture, if one service fails, it doesn't necessarily bring down the entire system. Other services can continue to function. Modular monoliths are more vulnerable; an error in one module can impact the whole application. Distributed monoliths share this vulnerability due to their tight coupling.

```mermaid

graph LR
  microservices["fa:fa-cubes Microservices"] --> errorIsolation["fa:fa-shield-alt Error Isolation"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> sharedRisk["fa:fa-exclamation-triangle Shared Risk"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> sharedRisk
```


## Data Consistency

Microservices face challenges in maintaining data consistency across services due to distributed transactions and eventual consistency patterns. Modular monoliths and distributed monoliths, with shared databases, can enforce consistency more easily but may face challenges in scaling and maintaining clear boundaries.

```mermaid
graph LR
  microservices["fa:fa-cubes Microservices"] --> eventualConsistency["❄️  Eventual Consistency"]
  modularMonolith["fa:fa-layer-group Modular Monolith"] --> strongConsistency["🪨 Strong Consistency"]
  distributedMonolith["fa:fa-network-wired Distributed Monolith"] --> strongConsistency
```

## How to Choose the Right Architecture

There is no definitive "best" choice among microservices, modular monoliths, and distributed monoliths. The optimal architecture depends on your project's specific requirements, team expertise, scalability needs, and long-term goals.

```mermaid

graph LR
  yourProject["fa:fa-project-diagram Your Project"] --> microservices["fa:fa-cubes Microservices"]
  yourProject --> modularMonolith["fa:fa-layer-group Modular Monolith"]
  yourProject --> distributedMonolith["fa:fa-network-wired Distributed Monolith"]
```

- **Consider the Human Factor:** Architecture decisions impact not just the code but also the people who work with it. Consider how the chosen architecture will affect your team's collaboration, productivity, and satisfaction.
- **Start Small:** Experiment with a small project or service to test the architecture's suitability before committing to a larger implementation.
- **Iterate and Improve:** Continuously evaluate and refine your architecture based on feedback and evolving project needs.
- **Stay Agile:** Be prepared to adapt and evolve your architecture as your project evolves. Agile methodologies can help you respond to changing requirements and refine your architecture iteratively.

# Keywords To Remember

```mermaid
graph 

  subgraph Characteristics["✨ "]
    teamAutonomy["fa:fa-users"]
    scalability["fa:fa-expand-arrows-alt"]
    flexibility["fa:fa-code"]
    deployment["fa:fa-rocket"]
    coupling["🔗"]
  end

  subgraph  
    microservices["fa:fa-cubes"]
    async["☕"]
    eventual-consistency["❄️"]
  end

  subgraph  
    distributedMonolith["fa:fa-network-wired"]
    sync["🕙"]
    strong-consistency["🪨"]
  end


  subgraph  [" "]
    modularMonolith["fa:fa-layer-group"]
    single["fa:fa-1"]
  end


```