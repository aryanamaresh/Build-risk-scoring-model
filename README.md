# Build-risk-scoring-model

📌 Overview :

This project assigns a risk score (0–1000) to Ethereum wallet addresses based on simulated DeFi activity, particularly inspired by on-chain behaviors in lending protocols like Compound V2/V3. The final score helps determine whether a wallet poses high, medium, or low risk.


📥 Data Collection Method:

Since on-chain data fetching (from Compound, Aave, etc.) via APIs like Covalent or DeFi SDKs wasn't implemented in this version, we used a simulation-based approach:

A CSV file containing wallet IDs was uploaded (Wallet id - Sheet1.csv)

For each wallet, synthetic DeFi behavior was generated using Python’s random module

Simulated features mimic real-world DeFi attributes like:

Total collateral supplied

Total tokens borrowed

Number of times liquidated

Borrowing diversity

Transaction frequency

Debt volatility

This approach allows prototyping the full risk scoring pipeline without relying on live API access.


📊 Feature Selection Rationale:

We selected six features, each chosen for its predictive power in identifying risky vs. safe wallets:

Feature	Why It's Important
Total Supplied	Indicates how much collateral the user has deposited
Total Borrowed	Reflects the user's exposure and debt size
Collateral Ratio (supplied / borrowed)	A low ratio means high leverage, which is riskier
Number of Liquidations	Frequent liquidations signal irresponsible borrowing
Borrowed Asset Diversity	Diversifying borrowed assets may indicate safer behavior

🧮 Scoring Method :

We use a rule-based (heuristic) scoring system, starting each wallet at a perfect score of 1000, then applying penalties and bonuses based on its behavior.

Penalties (Higher Risk → Lower Score)
150 points deducted for every liquidation

Up to 300 points deducted for poor collateral ratio

Up to 200 points deducted for high debt volatility

Bonuses (Lower Risk → Higher Score)
5 points per unit of average transaction frequency

20 points per unique token borrowed (diversity)

Final scores are clipped to remain in the 0–1000 range.
Average Transaction Frequency	More activity can imply a better-managed wallet
Debt Volatility	Highly fluctuating debt is a red flag for instability

All numeric features were normalized to a 0–1 scale using MinMaxScaler to ensure equal contribution during scoring.

⚠️ Justification of Risk Indicators:

Risk Indicator	Justification
Liquidations	Indicate a failure to repay loans or maintain safe collateral, thus strongly penalized
Collateral Ratio	Lower values indicate higher leverage, increasing the chance of liquidation
Debt Volatility	Fluctuating debt suggests poor financial management or instability
Activity Frequency	Active wallets are typically monitored and managed more frequently
Borrowing Diversity	Spreading risk across assets can reduce exposure to individual token crashes


📤 Output:

The final output is a CSV file containing:

All input and simulated metrics

Normalized features

Calculated risk_score (0–1000)

📁 File: Wallet id - Sheet1.csv


