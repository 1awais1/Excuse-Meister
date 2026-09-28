# 🗺️ Excuse-Meister — App Flowchart

Full interactive flow of the Excuse-Meister application.

---

## Main App Flow

```mermaid
flowchart TD
    A([User opens index.html]) --> B[Page loads\nLeaderboard reads from localStorage]

    B --> C{Pick a Category}
    C --> W[💼 Work]
    C --> S[🎒 School]
    C --> SO[🎉 Social]

    W  --> D[Scenario chips appear\nStep dot 1 turns green]
    S  --> D
    SO --> D

    D --> E{Pick a Scenario}

    E --> E1[Late to work / Late to class / Running late]
    E --> E2[Skipping a meeting / Skipping class / Cancelling plans]
    E --> E3[Missing a deadline / Forgot homework / Not texting back]
    E --> E4[Not replying / Failed a test / Leaving a date early]
    E --> E5[Leaving early / No pen-supplies / Dodging an event]

    E1 & E2 & E3 & E4 & E5 --> F[Step dot 2 turns green\nScenario label updates]

    F --> G([Click ⚡ Generate Excuse])

    G --> H{Category AND\nScenario selected?}
    H -- No --> I[Show nudge message\nask user to complete steps]
    I --> C

    H -- Yes --> J[Pick random excuse\nfrom scenario pool\navoid immediate repeat]

    J --> K[Display excuse in card\nStep dot 3 turns green]
    K --> L[Strength Meter fires\nRandom score 1-100]

    L --> M{Score range}
    M -- 1-33  --> M1[😇 Believable\nGreen bar]
    M -- 34-66 --> M2[😏 Petty\nOrange bar]
    M -- 67-100 --> M3[🤡 Ridiculous\nRed bar]

    M1 & M2 & M3 --> N[Copy and Upvote buttons enabled]

    N --> O{User action}
    O --> P[📋 Copy\nWrite to clipboard\nButton flashes Copied for 1.5s]
    O --> Q[👍 Upvote\nIncrement vote count\nSave to localStorage]
    O --> R[⚡ Generate again\nNew random excuse\nNew strength score]

    Q --> T[Re-render Leaderboard\nTop 5 by vote count\nRanked with medals]
    T --> U([User views\n🏆 Leaderboard panel])
```

---

## Leaderboard Sub-Flow

```mermaid
flowchart TD
    A([User clicks 👍 Upvote]) --> B[Increment vote count\nfor current excuse text]
    B --> C[Write updated votes object\nto localStorage as JSON]
    C --> D[Call renderLeaderboard]
    D --> E[Read all entries from votes object]
    E --> F[Sort descending by vote count]
    F --> G[Slice top 5]
    G --> H{Any votes exist?}
    H -- No  --> I[Show empty state message]
    H -- Yes --> J[Render ranked list\nwith medal emojis and vote badges]
    J --> K([Leaderboard panel updated])
```

---

## Data Flow

```mermaid
flowchart LR
    A[scenarios object\nin JS] -->|pool lookup| B[generateExcuse]
    B -->|currentExcuse| C[excuseBox DOM]
    B -->|random score| D[updateMeter]
    D -->|width + colour| E[meterFill DOM]
    C -->|upvoteExcuse| F[votes object]
    F -->|JSON.stringify| G[(localStorage\nexcuseMeisterVotes)]
    G -->|JSON.parse on load| F
    F -->|renderLeaderboard| H[leaderboardList DOM]
```

---

*See [`README.md`](README.md) for project overview · [`USER_GUIDE.md`](USER_GUIDE.md) for usage instructions.*
