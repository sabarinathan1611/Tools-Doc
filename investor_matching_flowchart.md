# Investor Matching System - Data Flow Flowchart

```mermaid
flowchart TD
    A[User Request] --> B{Authentication Check}
    B -->|Invalid| C[Return 401 Error]
    B -->|Valid| D{Pipeline Context?}

    D -->|Yes| E[Fetch Pipeline Data]
    E --> F{Has Pipeline Access?}
    F -->|No| G[Return Access Denied]
    F -->|Yes| H[Return Pipeline Investors]

    D -->|No| I{Force Refresh?}
    I -->|No| J{Existing Matches?}
    J -->|Yes| K{Free User?}
    K -->|Yes| L[Return Preview - 250 matches]
    K -->|No| M[Return Paginated Results]

    I -->|Yes| N[Check Refresh Limits]
    J -->|No| O[Get User Profile]
    N --> O

    O --> P{Profile Source Priority}
    P --> Q[Company Profile]
    P --> R[User Profile]
    P --> S[Create Default Profile]

    Q --> T{Valid Industry and Stage?}
    R --> T
    S --> T
    T -->|No| U[Return Error - Incomplete Profile]

    T -->|Yes| V[GPT Industry Expansion]
    V --> W[Fetch Investors from DB]
    W --> X{GPT Matching Available?}

    X -->|Yes| Y[GPT Intelligent Matching]
    Y --> Z[Filter by GPT Criteria]
    Z --> AA[Score and Rank Results]

    X -->|No| BB[Algorithmic Matching]
    BB --> CC[Calculate Match Scores]
    CC --> AA

    AA --> DD[Apply Score Thresholds]
    DD --> EE[Merge with Imported Contacts]
    EE --> FF[Remove Duplicates]
    FF --> GG[Apply User Filters]
    GG --> HH{User Access Level}

    HH -->|Free| II[Return Preview Results]
    HH -->|Paid| JJ[Apply Pagination]
    JJ --> KK[Return Full Results]

    II --> LL[Final Response]
    KK --> LL
    H --> LL
    L --> LL
    M --> LL
```

## Deep Algorithm Analysis - How Matching Works

```mermaid
flowchart TD
    subgraph "Data Collection Phase"
        A1[User Profile Data] --> A2[Industry: SaaS/Fintech/AI]
        A1 --> A3[Stage: Seed/Series-A/Growth]
        A1 --> A4[Funding Goal: $100K-$10M]
        A1 --> A5[Location: US/Europe/Asia]
        A1 --> A6[Business Description: Text]
        A1 --> A7[Investment Type: Equity/Convertible]
    end

    subgraph "AI Enhancement Phase"
        B1[GPT Industry Expansion] --> B2[Expand SaaS to: Enterprise Software, B2B, Cloud]
        B1 --> B3[Expand Fintech to: Payments, Banking, InsurTech]
        B1 --> B4[Expand AI to: Machine Learning, Data Analytics, Automation]
        B5[GPT Intelligent Matching] --> B6[Analyze 20,000 Investors]
        B6 --> B7[Extract Investment Patterns]
        B7 --> B8[Generate Match Criteria]
    end

    subgraph "Algorithmic Matching Engine"
        C1[Fetch 20,000 Active Investors] --> C2[Exclude Bounced Emails]
        C2 --> C3[For Each Investor: Calculate Score]

        C3 --> D1[Industry Matching - 40 pts max]
        D1 --> D11[Direct Match: 40 pts]
        D1 --> D12[Related Match: 35 pts]
        D1 --> D13[Expanded Match: 30 pts]
        D1 --> D14[Keyword Match: 25 pts]

        C3 --> D2[Stage Matching - 30 pts max]
        D2 --> D21[Direct Stage: 30 pts]
        D2 --> D22[Related Stage: 20 pts]
        D2 --> D23[Missing Data: 5 pts]

        C3 --> D3[Geographic Matching - 25 pts max]
        D3 --> D31[Same Region: 15 pts]
        D3 --> D32[Preferred Location: 10 pts]

        C3 --> D4[Funding Matching - 20 pts max]
        D4 --> D41[Sweet Spot Range: 20 pts]
        D4 --> D42[Close Range: 10 pts]

        C3 --> D5[Investment Type - 10 pts max]
        D5 --> D51[Type Match: 10 pts]
    end

    subgraph "Scoring & Filtering"
        E1[Total Score Calculation] --> E2[Min Score: 30 pts]
        E2 --> E3[Max Score: 100 pts]
        E3 --> E4[Sort by Score Descending]
        E4 --> E5[Limit to 11,000 Results]
    end

    subgraph "Data Merging & Access Control"
        F1[Merge with Imported Contacts] --> F2[Remove Duplicates]
        F2 --> F3[Prioritize Imported Contacts]
        F3 --> F4{User Type Check}
        F4 -->|Free User| F5[Preview: 250 investors with full details]
        F4 -->|Paid User| F6[Full Access: 12 per page, all details]
        F5 --> F7[Save to Cache - 25,000 limit]
        F6 --> F7
    end

    subgraph "Fallback Mechanisms"
        G1{GPT Available?} -->|No| G2[Algorithmic Matching Only]
        G1 -->|Yes| G3{GPT Success?}
        G3 -->|No| G2
        G3 -->|Yes| G4[GPT Enhanced Matching]
        G2 --> G5[Basic Score Calculation]
        G4 --> G5
        G5 --> G6{Results Found?}
        G6 -->|No| G7[Return Empty with Message]
        G6 -->|Yes| G8[Return Matches]
    end

    A2 --> B1
    A3 --> B5
    A4 --> D4
    A5 --> D3
    A6 --> B1
    A7 --> D5

    B2 --> D1
    B3 --> D1
    B4 --> D1
    B8 --> C3

    D11 --> E1
    D12 --> E1
    D13 --> E1
    D14 --> E1
    D21 --> E1
    D22 --> E1
    D23 --> E1
    D31 --> E1
    D32 --> E1
    D41 --> E1
    D42 --> E1
    D51 --> E1

    E5 --> F1
    F7 --> G1
```

### Key Algorithm Details:

**Data Points Collected:**

- Industry (with GPT expansion to related sectors)
- Investment Stage (Pre-seed, Seed, Series-A, etc.)
- Funding Goal (with sweet spot matching)
- Geographic Location (region and preferred locations)
- Business Description (used for AI analysis)
- Investment Type (Equity, Convertible, etc.)

**Matching Process:**

- **Primary Method**: GPT Intelligent Matching (analyzes 20,000 investors)
- **Fallback Method**: Algorithmic scoring system (125 points max)
- **Investor Database**: 20,000 active investors (excludes bounced emails)
- **Score Thresholds**: Minimum 30 points, Maximum 100 points
- **Result Limits**: Up to 11,000 matches, cached up to 25,000

**Fallback Mechanisms:**

- GPT failure → Algorithmic matching
- Missing profile → Default SaaS + Seed profile
- No results → Return empty with helpful message
- Database error → Return cached data
- Invalid profile → Return error with guidance
