# FraudGuardAI - Intelligent Financial Fraud Detection using LLM and RAG

FraudGuardAI is an innovative fraud detection and regulatory compliance system that leverages the power of Large Language Models (LLMs) and Retrieval-Augmented Generation (RAG) to provide real-time fraud detection, context-aware transaction monitoring, and automated regulatory compliance checks.

# Flow Diagram
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

## Probable Data Structure (Initial perception)

```mermaid
erDiagram
    CUSTOMER ||--o{ TRANSACTION : has
    CUSTOMER {
        string customer_id PK
        string name
        date date_of_birth
        string email
        string phone
        string address
        date customer_since
        string account_type
        float risk_score
        string kyc_status
        date kyc_last_update
    }
    TRANSACTION {
        string transaction_id PK
        string customer_id FK
        string type
        float amount
        string currency
        datetime timestamp
        string status
        string channel
        string source_account
        string destination_account
        string description
        string ip_address
        float risk_score
        boolean flagged
    }
    CUSTOMER ||--o{ KYC_DOCUMENTS : has
    KYC_DOCUMENTS {
        string document_id PK
        string customer_id FK
        string document_type
        string document_number
        date expiry_date
        string verification_status
    }
    CUSTOMER ||--o{ BUSINESS_INFO : has
    BUSINESS_INFO {
        string business_id PK
        string customer_id FK
        string registration_number
        string industry_type
        string company_size
        string revenue_range
        json beneficial_owners
    }
```
## Data Pipeline Architecture

```mermaid
flowchart LR
    subgraph Sources ["Data Sources"]
        direction TB
        TD["Transaction Data<br/>(CSV/JSON)"]
        CD["Customer Data<br/>(Database)"]
        FP["Fraud Patterns<br/>(JSON/YAML)"]
        RD["Regulatory Docs<br/>(PDF/Text)"]
    end

    subgraph Processing ["Data Processing Pipeline"]
        direction TB
        subgraph TP ["Transaction Processing"]
            TL["Data Loading"]
            TC["Cleaning"]
            TF["Feature Extraction"]
        end
        
        subgraph CP ["Customer Processing"]
            CL["Data Loading"]
            CC["Cleaning"]
            CF["Feature Extraction"]
        end
        
        subgraph FPP ["Fraud Pattern Processing"]
            FPL["Pattern Loading"]
            FPC["Pattern Conversion"]
        end
        
        subgraph RP ["Regulatory Processing"]
            RL["Document Loading"]
            RT["Text Extraction"]
            RE["Embedding Generation"]
        end
    end

    subgraph Output ["Processed Data"]
        direction TB
        CTF["Clean Transaction Features"]
        CCF["Clean Customer Features"]
        FPV["Fraud Pattern Vectors"]
        RDV["Regulatory Doc Vectors"]
    end

    TD --> TL --> TC --> TF --> CTF
    CD --> CL --> CC --> CF --> CCF
    FP --> FPL --> FPC --> FPV
    RD --> RL --> RT --> RE --> RDV

    classDef source fill:#2563eb,stroke:#1d4ed8,stroke-width:2px,color:#fff
    classDef process fill:#059669,stroke:#047857,stroke-width:2px,color:#fff
    classDef output fill:#dc2626,stroke:#b91c1c,stroke-width:2px,color:#fff
    
    class TD,CD,FP,RD source
    class TL,TC,TF,CL,CC,CF,FPL,FPC,RL,RT,RE process
    class CTF,CCF,FPV,RDV output
```