# CapitalReach.ai Tools - Flowcharts

## SaaS Valuation Calculator Flowchart

```mermaid
flowchart TD
    A[Start] --> B{Stage Selection}
    B -->|Pre-Revenue| C[Pre-Revenue Calculation]
    B -->|Post-Revenue| D[Post-Revenue Calculation]

    C --> C1[Check Prototype]
    C1 --> C2[Add Team Experience Score]
    C2 --> C3[Add Partnerships Score]
    C3 --> C4[Add Early Users Score]
    C4 --> C5[Calculate Total]
    C5 --> C6[Apply ±10% Range]
    C6 --> E[Return Result]

    D --> D1[Calculate ARR]
    D1 --> D2[Calculate Net Retention]
    D2 --> D3[Apply Core Formula]
    D3 --> D4{Check Gross Margin}
    D4 -->|>80%| D5[+10% Adjustment]
    D4 -->|<60%| D6[-10% Adjustment]
    D4 -->|60-80%| D7[No Adjustment]
    D5 --> D8[Apply ARR Caps]
    D6 --> D8
    D7 --> D8
    D8 --> D9[Apply ±10% Range]
    D9 --> E

    E --> F[Store in Database]
    F --> G[End]
```

## Co-Founder Equity Split Flowchart

```mermaid
flowchart TD
    A[Start] --> B[Input Founder Data]
    B --> C[Score Each Founder]
    C --> D[Calculate Total Points per Founder]
    D --> E[Sum All Founder Points]
    E --> F{Total > 0?}
    F -->|No| G[Equal Split]
    F -->|Yes| H[Calculate Raw Percentages]
    G --> I[50/50 Split]
    H --> J[Round Percentages]
    J --> K[Sum Rounded Percentages]
    K --> L{Sum = 100%?}
    L -->|Yes| M[Return Results]
    L -->|No| N[Adjust Last Founder]
    N --> M
    I --> M
    M --> O[Store in Database]
    O --> P[End]
```

## Quick Reference: Calculation Methods

### SaaS Valuation - Pre-Revenue

```
Valuation = Sum of Factors:
- Prototype: +$500K (if yes)
- Team Experience: +$100K-$500K
- Partnerships: +$300K (if yes)
- Early Users: +$200K (if >100 users)
```

### SaaS Valuation - Post-Revenue

```
Valuation = ARR × Growth × Net Retention × 10
Where:
- ARR = MRR × 12
- Net Retention = 1 - Churn Rate
- Apply margin adjustments (±10%)
- Cap between 3× and 15× ARR
```

### Equity Split

```
For each founder:
1. Score 0-5 in each category
2. Sum all scores per founder
3. Calculate percentage: (Founder Total / All Totals) × 100
4. Round and ensure sum = 100%
```

---

_These flowcharts visualize the decision logic and calculation processes for both tools._
