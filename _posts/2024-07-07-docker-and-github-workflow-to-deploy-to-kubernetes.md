---
title: Docker and GitHub Workflow to Deploy to Kubernetes
categories: [tech]
date: 2024-07-07 00:00:00
tags: [docker, kubernetes, gitHub workflow ]
image: "/assets/images/docker-and-github-workflow-to-deploy-to-kubernetes.png"
---

This is the workflow I use to build and push Docker images to AWS ECR and deploy to Kubernetes using Helm. The workflow is triggered by a push to the master branch. It checks the services that have changed and calls the appropriate workflow to build and push the Docker images.


## Project Directory Structure

```bash
main-repo/
├── .github/
│   └── workflows/
│       ├── build-push.yml
│       └── main.yml
├── serviceA/
│   ├── build/
│   │   └── Dockerfile
│   ├── helm/
│   └── Makefile
└── serviceB/
    ├── build/
    │   └── Dockerfile
    ├── helm/
    └── Makefile

```

## Dockerfile


```bash
FROM node:18-alpine
WORKDIR /usr/app
COPY ./package.json ./
COPY ./yarn.lock ./
RUN yarn install --production
COPY ./ ./
CMD ["yarn", "start"]
```

```mermaid
graph TD
    BaseImage["Base Image: FROM node:14"]
    WorkDirectory["Working Directory: WORKDIR /app"]
    CopyFiles["Copy Files: COPY package*.json ./"]
    InstallDependencies["Run Commands: RUN npm install"]
    CopyApp["Copy Application Files: COPY . ./"]
    ExposePorts["Expose Ports: EXPOSE 3000"]
    StartCommand["Default Command: CMD ['node', 'app.js']"]

    BaseImage --> WorkDirectory
    WorkDirectory --> CopyFiles
    CopyFiles --> InstallDependencies
    InstallDependencies --> CopyApp
    CopyApp --> ExposePorts
    ExposePorts --> StartCommand

    style BaseImage fill:#f96,stroke:#333,stroke-width:2px
    style WorkDirectory fill:#69c,stroke:#333,stroke-width:2px
    style CopyFiles fill:#f69,stroke:#333,stroke-width:2px
    style InstallDependencies fill:#96c,stroke:#333,stroke-width:2px
    style CopyApp fill:#c96,stroke:#333,stroke-width:2px
    style ExposePorts fill:#6c9,stroke:#333,stroke-width:2px
    style StartCommand fill:#96f,stroke:#333,stroke-width:2px
```
## Contents of a Docker container

```mermaid
graph
    subgraph Container["Docker Container /usr/app"]
        AppCode["Application Code (Copied from ./)"]
        PackageJSON["package.json (Copied)"]
        YarnLock["yarn.lock (Copied)"]
        NodeModules["node_modules (Created by yarn install)"] 
    end
    
    style Container fill:#f0f8ff,stroke:#007bff,color:black
    style AppCode fill:#e9ecef
    style PackageJSON fill:#e9ecef
    style YarnLock fill:#e9ecef
    style NodeModules fill:#e9ecef
```

## Github Workflow

```mermaid
graph TD
    subgraph workflowTrigger["Workflow Trigger"]
        pushEvent["Push Event"]
        pullRequestEvent["Pull Request Event"]
        scheduleEvent["Schedule Event"]
        manualEvent["Manual Event"]
        externalEvent["External Event"]
    end

    workflowTrigger --> jobExecution["**Job Execution**"]

    subgraph jobExecution["Job Execution"]
        job1["Job 1"]
        job2["Job 2"]
        job3["Job 3"]
    end

    jobExecution --> workflowStatus["**Workflow Status**"]

    subgraph workflowStatus["Workflow Status"]
        success["Success"]
        failure["Failure"]
        cancelled["Cancelled"]
    end

    style workflowTrigger fill:#f9f2e8,stroke:#bf8040
    style jobExecution fill:#e8f0fe,stroke:#406cbf
    style workflowStatus fill:#e6f2ea,stroke:#2e8b57
    style pushEvent fill:#fed8b1,stroke:#bf8040
    style pullRequestEvent fill:#d1e0e0,stroke:#406cbf
    style scheduleEvent fill:#b19cd9,stroke:#4b0082
    style manualEvent fill:#ffffe0,stroke:#8b4513
    style externalEvent fill:#ccffcc,stroke:#006400
    style success fill:#90ee90,stroke:#2e8b57
    style failure fill:#f08080,stroke:#8b0000
    style cancelled fill:#add8e6,stroke:#00008b


```

- **Workflow Trigger**: Represents the event that initiates the workflow (e.g., push, pull request, schedule, manual, external).
- **Job Execution**: The individual jobs within the workflow are executed in parallel or sequentially.
- **Workflow Status**: The final outcome of the workflow (success, failure, or cancellation).

## Workflow Chaining


### Main Workflow
```mermaid
graph TD
    subgraph mainWorkflow["Main Workflow"]
        workflowTrigger["Push Event on Master Branch"] --> getServicesName["Get Services Name Job"]
    end

    getServicesName["Get Services Name Job"] -->|If existing services changed| buildAndPush["Call Build & Push Workflow "]
    getServicesName["Get Services Name Job"] -->|If new services added| buildAndPushInstall["Call Build & Push Workflow (with Install) "]

    style mainWorkflow fill:#f9f2e8,stroke:#bf8040
    style workflowTrigger fill:#fed8b1,stroke:#bf8040
    style getServicesName fill:#d1e0e0,stroke:#406cbf
    style buildAndPush fill:#e6f2ea,stroke:#2e8b57
    style buildAndPushInstall fill:#d1e0e0,stroke:#406cbf

```
The main workflow is triggered by a push to the master. Base on the changes in the services, it calls the appropriate workflow to build and push the Docker images.

### Build & Push Workflow
```mermaid
graph TD
    subgraph buildAndPushWorkflow["Build & Push Workflow"]
        buildAndPushJob["Build and Push Job"]

        subgraph preparation["Preparation"]
            init["Set Service Name, etc."]
            configureAWSCredentials["Configure AWS Credentials"]
            checkoutCode["Checkout Code"]
        end

        subgraph build["Build and Deploy"]
            bumpVersion["Bump Version"]
            buildAndPushImage["Build & Push Docker Image"]
            bumpHelm["Bump Helm"]
            autoCommit["Git Auto Commit"]
            helmDeploy["Helm Deploy (if deploy=true)"]
        end
    end

    style buildAndPushWorkflow fill:#e8f0fe,stroke:#406cbf
    style buildAndPushJob fill:#e6f2ea,stroke:#2e8b57
    style preparation fill:#b19cd9,stroke:#4b0082
    style build fill:#ffffe0,stroke:#8b4513
```


