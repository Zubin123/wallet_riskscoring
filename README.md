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

3. **Normalize to 0–1000 Range**  
Each wallet’s raw score is converted into a percentile rank and scaled:
final_score = percentile_rank(score_raw) * 1000


This ensures a consistent, relative scoring between 0 (highest risk) and 1000 (lowest risk).

---

## 🔍 Justification of Risk Indicators

- **Activity & Engagement**: High tx count and approvals suggest real users, not dormant or malicious wallets.
- **Diversity**: Interacting with multiple tokens implies broader exposure and better understanding of protocol mechanics.
- **Economic Commitment**: More gas spent usually reflects deeper protocol engagement.
- **Consistency**: Long-term users are generally more reliable and less opportunistic.

---

## 🧱 Scalability

- **Protocol-Agnostic**: Can be extended to Aave, Uniswap, etc., with minimal modifications.
- **Transparent**: Interpretable features and scoring logic support human-in-the-loop audits.
- **Expandable**: Can be upgraded using ML models or clustering for advanced risk profiling.

---

## ✅ Output

Final output is a DataFrame with wallet IDs and scores:

| wallet_id | score  |
|-----------|--------|
| 0x123...  | 742.4  |
| 0xabc...  | 315.2  |
| ...       | ...    |

This score can be used in credit allocation, trust modeling, or DeFi onboarding strategies.

---

## 👨‍💻 Author

Internship Submission – Round 2  
**Wallet Risk Scoring Based on Compound Protocol**  
*Submitted by: Mohammed Zubin Essudeen*
