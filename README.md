# FraudGuardAI - Intelligent Financial Fraud Detection using LLM and RAG

FraudGuardAI is an innovative fraud detection and regulatory compliance system that leverages the power of Large Language Models (LLMs) and Retrieval-Augmented Generation (RAG) to provide real-time fraud detection, context-aware transaction monitoring, and automated regulatory compliance checks.


```mermaid
flowchart TB
    subgraph Input ["Data Sources"]
        TD["Transaction Data"]
        CD["Customer Data"]
        FP["Fraud Patterns DB"]
        RD["Regulatory Docs"]
    end
    subgraph Processing ["Data Processing"]
        direction LR
        DP["Pandas/Dask Pipeline"]
        FE["Feature Engineering"]
        RS["Redis Pub/Sub"]
    end
    subgraph AI ["AI Layer"]
        direction LR
        subgraph Models ["Model Pipeline"]
            TF["Transaction Encoder<br/>(RoBERTa)"]
            TB["Temporal BERT"]
        end
        
        subgraph RAG ["RAG System"]
            VS["Vector Store<br/>(Pinecone)"]
            RC["RAG Chain"]
            CA["Context Augmentation"]
        end
        
        subgraph LLM ["LLM Pipeline"]
            CL["Claude API"]
            PE["Prompt Engineering"]
            CH["Chain of Thought"]
        end
    end
    subgraph Backend ["Application Backend"]
        direction LR
        FL["Flask API"]
        RQ["Redis Queue"]
    end
    subgraph Frontend ["Streamlit Application"]
        direction LR
        SD["Streamlit Dashboard"]
        subgraph Components ["Dashboard Components"]
            MP["Monitoring Panel"]
            AP["Analytics Views"]
            RP["Real-time Alerts"]
        end
    end
    TD --> DP
    CD --> DP
    FP --> VS
    RD --> VS
    
    DP --> FE
    FE --> RS
    
    RS --> Models
    Models --> VS
    VS --> RC
    RC --> CA
    CA --> CL
    
    CL --> PE
    PE --> CH
    
    CH --> FL
    FL --> RQ
    RQ --> RS
    
    FL --> SD
    SD --> MP
    SD --> AP
    SD --> RP
    
    classDef primary fill:#2563eb,stroke:#1d4ed8,stroke-width:2px,color:#fff
    classDef secondary fill:#4b5563,stroke:#374151,stroke-width:2px,color:#fff
    classDef modern fill:#059669,stroke:#047857,stroke-width:2px,color:#fff
    classDef frontend fill:#dc2626,stroke:#b91c1c,stroke-width:2px,color:#fff
    class TD,CD,FP,RD primary
    class DP,FE,RS,FL,RQ secondary
    class TF,TB,VS,RC,CA,CL,PE,CH modern
    class SD,MP,AP,RP frontend
```