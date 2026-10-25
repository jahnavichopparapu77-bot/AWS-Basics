# AWS-Basics
Hands-on AWS learning repository covering cloud computing fundamentals, core AWS services, networking, IAM, storage, and practical labs with documentation.

# AWS Architecture

## AWS Request Flow

```mermaid
flowchart TB
    U["📱<br/>1. User / Client<br/><small>Browser, mobile app, or another application sends an HTTP request.</small>"]

    R53["2. Amazon Route 53<br/><small>DNS service that directs the domain to the appropriate endpoint.</small>"]

    CF["3. Amazon CloudFront<br/><small>CDN that serves cached content when available, or forwards requests to the configured origin.</small>"]

    API["4. Amazon API Gateway<br/><small>Accepts API calls, applies configured authorization and throttling, and routes requests to a backend integration.</small>"]

    subgraph COMPUTE[" "]
        direction LR
        EC2["▤<br/><b>EC2</b><br/><small>Virtual servers</small>"]
        ECS["⬡<br/><b>ECS</b><br/><small>Containerized apps</small>"]
        LAM["ϟ<br/><b>Lambda</b><br/><small>Event-driven functions</small>"]
    end

    NOTE["Choose a suitable backend architecture. These are alternatives in many designs, not services every request must pass through."]

    subgraph DATA[" "]
        direction TB
        subgraph DATABASES[" "]
            direction LR
            RDS["▤<br/><b>RDS</b><br/><small>Relational database: tables and SQL</small>"]
            DDB["▤<br/><b>DynamoDB</b><br/><small>Managed NoSQL database</small>"]
        end
        subgraph STORAGE[" "]
            direction LR
            S3["▤<br/><b>S3</b><br/><small>Objects, images, files, backups</small>"]
            SQS["♧<br/><b>SQS</b><br/><small>Asynchronous message processing</small>"]
        end
    end

    subgraph SUPPORT[" "]
        direction LR
        CW["▣<br/><b>CloudWatch</b><br/><small>Logs, metrics, alarms</small>"]
        IAM["♢<br/><b>IAM</b><br/><small>Identity and permissions</small>"]
    end

    FOOT["CloudWatch observes your system; IAM controls access. They support the architecture rather than forming sequential request-processing steps."]

    U --> R53
    R53 --> CF
    CF -->|"For API requests, if configured this way"| API
    API --> EC2
    API --> ECS
    API --> LAM

    EC2 ~~~ NOTE
    ECS ~~~ NOTE
    LAM ~~~ NOTE
    NOTE ~~~ RDS

    RDS ~~~ S3
    DDB ~~~ SQS
    S3 ~~~ CW
    SQS ~~~ IAM
    CW ~~~ FOOT
    IAM ~~~ FOOT

    classDef main fill:#e8f2ff,stroke:#d5e5fa,color:#222,stroke-width:1px
    classDef neutral fill:#ffffff,stroke:#dedede,color:#222,stroke-width:1px
    classDef support fill:#e8f2ff,stroke:#d5e5fa,color:#222,stroke-width:1px
    classDef note fill:#ffffff,stroke:#ffffff,color:#666,stroke-width:0px

    class U,CW,IAM main
    class R53,CF,API,EC2,ECS,LAM,RDS,DDB,S3,SQS neutral
    class NOTE,FOOT note

    style COMPUTE fill:#ffffff,stroke:#ffffff
    style DATA fill:#ffffff,stroke:#ffffff
    style DATABASES fill:#ffffff,stroke:#ffffff
    style STORAGE fill:#ffffff,stroke:#ffffff
    style SUPPORT fill:#ffffff,stroke:#d5d5d5
```

## Component Summary

| AWS Component | Purpose |
|---|---|
| User / Client | Sends HTTP requests |
| Route 53 | DNS resolution |
| CloudFront | Content delivery and caching |
| API Gateway | API routing and access controls |
| EC2 | Virtual servers |
| ECS | Container orchestration |
| Lambda | Serverless functions |
| RDS | Relational database |
| DynamoDB | NoSQL database |
| S3 | Object storage |
| SQS | Asynchronous messaging |
| CloudWatch | Monitoring and logs |
| IAM | Identity and access permissions |
