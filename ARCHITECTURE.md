# Auto Highlight Extension Architecture

This document provides a visual overview of the Auto Highlight browser extension architecture using Mermaid diagrams.

## Extension Workflow

```mermaid
graph TD
    A[User Action] --> B{Action Type}
    B -->|Click Icon| C[Toggle Highlight]
    B -->|Context Menu| D[Select Highlight Level]
    B -->|Page Load| E[Autonomous Highlight]
    
    C --> F[Event Page/Background Script]
    D --> F
    E --> F
    
    F --> G{Check Permissions}
    G -->|Denied| H[Show Error]
    G -->|Granted| I[Inject Scripts]
    
    I --> J[Insert CSS]
    J --> K[Load Libraries]
    K --> L[readabilitySAX.js]
    K --> M[PorterStemmer.js]
    K --> N[nlp.js]
    K --> O[content.js]
    
    L --> P[Extract Article Content]
    M --> Q[Text Processing]
    N --> Q
    
    P --> R[Analyze Text]
    Q --> R
    
    R --> S{Highlight State}
    S -->|None| T[Remove Highlights]
    S -->|Partial| U[Highlight Key Content]
    S -->|Max| V[Highlight More Content]
    
    T --> W[Update Icon]
    U --> W
    V --> W
    
    W --> X[Display Results]
```

## Component Architecture

```mermaid
graph LR
    A[Browser Extension] --> B[Background Script]
    A --> C[Content Scripts]
    A --> D[Options Page]
    A --> E[Manifest Config]
    
    B --> B1[eventPage.js]
    B --> B2[shared.js]
    B --> B3[matchPattern.js]
    
    C --> C1[content.js]
    C --> C2[nlp.js]
    C --> C3[style.css]
    
    C1 --> L1[readabilitySAX]
    C2 --> L2[PorterStemmer]
    
    D --> D1[options.html]
    D --> D2[options.js]
    
    B1 -.-> |Message Passing| C1
    C1 -.-> |Update Icon| B1
    
    D1 -.-> |Storage API| B1
```

## State Management

```mermaid
stateDiagram-v2
    [*] --> None: Extension Loaded
    None --> Partial: Click Icon / Menu
    Partial --> Max: Click Icon / Menu
    Max --> None: Click Icon / Menu
    
    None --> Autonomous: Page Load
    Autonomous --> None: User Override
    Autonomous --> Partial: Configured State
    Autonomous --> Max: Configured State
    
    note right of Autonomous
        Triggered automatically
        based on blocklist/allowlist
    end note
```

## Data Flow

```mermaid
flowchart TB
    A[Web Page] --> B[Content Script Injected]
    B --> C[readabilitySAX Parser]
    C --> D[Extract DOM Nodes]
    
    D --> E[NLP Processing]
    E --> F[Tokenization]
    F --> G[Stopword Filtering]
    G --> H[Stemming via Porter]
    
    H --> I[Calculate Word Scores]
    I --> J[Score Sentences]
    J --> K[Rank Sentences]
    
    K --> L{Highlight Level}
    L -->|Level 0| M[No Highlighting]
    L -->|Level 1| N[Top Sentences]
    L -->|Level 2| O[More Sentences]
    L -->|Level 3| P[Maximum Coverage]
    
    M --> Q[Apply CSS Classes]
    N --> Q
    O --> Q
    P --> Q
    
    Q --> R[Render Highlights]
```
