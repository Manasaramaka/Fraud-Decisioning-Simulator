# Real-Time Fraud Decisioning Simulator

A Python-based fraud analytics simulation engine that models CNP and CP transaction decisioning across rule-based controls, score-based risk tiers, and false-positive optimization logic. Built to replicate the core decisioning layer used in production fraud strategy environments.

---

## Project Overview

Fraud strategy teams balance three competing pressures: catching fraud losses, minimizing false positives, and protecting approval rates. Most public projects demonstrate fraud *detection* , this project demonstrates fraud *decisioning*: the logic layer that determines what happens after a risk signal fires.

This simulator generates synthetic transaction streams, applies configurable rule and score-based controls, evaluates outcomes across fraud categories, and surfaces approval rate vs. fraud loss tradeoffs in an interactive dashboard.

---

## Key Features

- **CNP/CP Transaction Simulation** - Generates card-present and card-not-present transaction streams with configurable fraud injection rates across merchant categories, geographies, and velocity patterns
- **Rule-Based Control Engine** - Applies velocity rules, amount thresholds, merchant-risk flags, geographic anomaly rules, and post-authorization monitoring logic with configurable parameters
- **Score-Based Decisioning Layer** - Assigns risk scores using Gradient Boosting and Logistic Regression models; routes transactions to approve, review, or decline queues based on score thresholds
- **False Positive Analyzer** - Tracks false positive rate, review queue volume, and customer friction impact across rule configurations; supports threshold sensitivity analysis
- **Approval Rate Monitor** - Measures approval rate impact of each rule and score threshold change; surfaces the fraud loss vs. approval rate tradeoff curve
- **Fraud Loss Calculator** - Estimates fraud loss exposure by category (CNP, velocity abuse, dispute-linked) under each decisioning configuration
- **Interactive Dashboard** - Streamlit UI for adjusting rule parameters, score thresholds, and fraud injection rates in real time with live metric updates

---

## Fraud Categories Modeled

| Category | Signals Simulated |
|---|---|
| CNP Fraud | Device mismatch, billing/shipping gap, high-risk MCC, velocity |
| Card-Present Fraud | Fallback transactions, PIN bypass attempts, unusual POS patterns |
| Velocity Abuse | Rapid sequential transactions, split-amount patterns, burst activity |
| Dispute-Linked Risk | Repeat dispute history, merchant chargeback rate, refund abuse |

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data Simulation | Python, NumPy, Faker |
| Rule Engine | Python, Pandas |
| Scoring Models | Scikit-learn (Gradient Boosting, Logistic Regression) |
| Dashboard | Streamlit, Plotly |
| Data Storage | SQLite (local), CSV export |
| Version Control | Git, GitHub |

---

## Project Structure

```
fraud-decisioning-simulator/
│
├── data/
│   ├── synthetic_transactions.csv       # Generated transaction dataset
│   └── fraud_labels.csv                 # Ground truth fraud labels
│
├── engine/
│   ├── transaction_generator.py         # CNP/CP transaction stream generator
│   ├── rule_engine.py                   # Velocity, threshold, and risk-flag rules
│   ├── score_engine.py                  # Gradient Boosting and LR scoring models
│   └── decisioning_router.py            # Approve / review / decline routing logic
│
├── analysis/
│   ├── false_positive_analyzer.py       # FP rate, queue volume, friction metrics
│   ├── approval_rate_monitor.py         # Approval rate impact by rule/threshold
│   └── fraud_loss_calculator.py         # Loss exposure by fraud category
│
├── dashboard/
│   └── app.py                           # Streamlit interactive dashboard
│
├── notebooks/
│   └── strategy_analysis.ipynb          # Exploratory analysis and threshold tuning
│
├── requirements.txt
└── README.md
```

---

## How It Works

**Step 1 - Generate Transactions**
The transaction generator produces a configurable volume of CNP and CP transactions with embedded fraud patterns. Fraud injection rate, merchant category mix, and velocity parameters are all adjustable.

**Step 2 - Apply Rule-Based Controls**
The rule engine evaluates each transaction against a configurable ruleset. Rules fire independently and feed into a composite risk flag. Rule performance metrics (hit rate, FP rate, approval impact) are tracked per rule.

**Step 3 - Score Transactions**
Flagged and unflagged transactions pass through the scoring layer. A Gradient Boosting model assigns a risk score based on transaction features. Score thresholds determine queue routing.

**Step 4 - Route Decisions**
The decisioning router assigns each transaction to approve, manual review, or decline based on rule flags and score thresholds. Threshold sensitivity is adjustable in real time via the dashboard.

**Step 5 - Evaluate Outcomes**
The analysis layer calculates: false positive rate, fraud loss exposure, approval rate impact, review queue volume, and the fraud loss vs. approval rate tradeoff curve across all configuration scenarios.

---

## Key Metrics Tracked

- **Fraud Detection Rate** - % of true fraud transactions caught
- **False Positive Rate** - % of legitimate transactions incorrectly flagged
- **Approval Rate** - % of total transactions approved under each configuration
- **Review Queue Volume** - Number of transactions routed to manual review
- **Estimated Fraud Loss** - Dollar exposure under each decisioning configuration
- **Precision / Recall by Category** - Per-category model performance

---

## Sample Output

```
Configuration: Velocity Rule (5 txn / 10 min) + Score Threshold 0.72
─────────────────────────────────────────────────────────
Transactions Simulated:        50,000
Fraud Injection Rate:          2.1%
Fraud Detected:                91.4%
False Positive Rate:           6.8%
Approval Rate:                 94.2%
Review Queue Volume:           3,247
Estimated Fraud Loss Avoided:  $142,300
─────────────────────────────────────────────────────────
```

---

## Setup & Usage

```bash
# Clone the repository
git clone https://github.com/Manasaramaka/fraud-decisioning-simulator.git
cd fraud-decisioning-simulator

# Install dependencies
pip install -r requirements.txt

# Generate synthetic transaction data
python engine/transaction_generator.py

# Run the interactive dashboard
streamlit run dashboard/app.py
```

---

## Relevance to Production Fraud Strategy

This project models the decisioning logic that sits between a fraud detection signal and a transaction outcome. The core tradeoffs — false positive rate vs. fraud detection rate, approval rate vs. loss exposure, rule precision vs. rule recall — are the same tradeoffs managed daily in production fraud strategy environments at banks, fintechs, and payment networks.

---

## Author

**Manasa Ramaka**
[LinkedIn](https://linkedin.com/in/manasaramaka) | [Portfolio](https://manasaramaka.github.io)
