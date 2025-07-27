# 🧠 Compound V2/V3 Wallet Risk Scoring

This project implements a wallet risk scoring system (0–1000) using behavioral transaction features from the Compound protocol (V2/V3). It is designed to evaluate the financial risk of wallets by analyzing historical interactions and assigning interpretable, data-driven credit scores.

---

## 📥 Data Collection Method

We collected on-chain transaction data using the **Covalent API**, which offers access to granular DeFi protocol activity per wallet address. The provided list of 100 wallets was used to extract:

- All historical Compound V2/V3 transactions
- Associated gas fees and tokens interacted with
- Event types such as `Borrow`, `Repay`, `Supply`, `Withdraw`, and `Approval`

The data was normalized and aggregated into a structured format (`features_df`) containing engineered features per wallet.

---

## 📊 Feature Selection Rationale

The selected features aim to reflect both **activity level** and **financial intent**, which are key proxies for wallet credibility:

| Feature | Rationale |
|--------|-----------|
| `total_compound_tx_count` | High activity generally signals deeper protocol engagement and familiarity. |
| `num_unique_tokens_interacted` | Indicates diversity of asset interaction, helping assess experience and versatility. |
| `total_gas_spent_usd` | Higher gas usage typically correlates with more serious, high-value operations. |
| `interaction_duration_days` | Long-term interaction suggests sustained usage and trustworthiness. |
| `num_approval_events` | Approvals signal preparation for usage; frequent approvals may imply proactive users. |

These features are **protocol-agnostic and extensible**, making the solution scalable to other DeFi platforms.

---

## 🧮 Scoring Method

1. **Standardization**  
   All selected features are standardized using `StandardScaler` to ensure uniform scale and fair weight assignment.

2. **Weighted Aggregation**  
   A raw score is calculated using a weighted sum:

   ```python
   score_raw = (
       0.2 * total_compound_tx_count +
       0.2 * num_unique_tokens_interacted +
       0.3 * total_gas_spent_usd +
       0.2 * interaction_duration_days +
       0.1 * num_approval_events
   )
3. **Ranking and Scaling**
   The raw scores are converted to percentile ranks and scaled to a 0–1000 range:
     score = percentile_rank(score_raw) * 1000