# Market-Risk-Analytics-Lab-using-Synthetic-Stock-Market-DataIndustry Use Case
Investment banks, hedge funds, mutual funds, and trading firms use market risk analytics to understand how market fluctuations impact investments and portfolios.

Market risk refers to the possibility of financial loss due to:

stock price volatility
market crashes
interest rate changes
inflation
economic events
geopolitical uncertainty
Lab Objective
In this lab, students will learn how to:

Create a synthetic stock market dataset.
Understand market risk indicators.
Analyze stock volatility and return patterns.
Classify stocks into Low, Medium, and High market risk categories.
Evaluate investment strategies based on market behavior.
Visualize market risk patterns.
Understand how AI supports investment analytics.
Real-World Relevance
This workflow is used in:

portfolio risk management
trading analytics
stock market forecasting
robo-advisory systems
wealth management
algorithmic trading systems
Dataset Note
This dataset is synthetic and created only for educational purposes.


[ ]
# CELL 1: Import required libraries

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

pd.set_option('display.max_columns', None)

print("Libraries imported successfully.")

[ ]
# CELL 2: Create stock names and sectors

np.random.seed(42)

stocks = [
    "Reliance", "TCS", "Infosys", "HDFC Bank", "ICICI Bank",
    "SBI", "Wipro", "Adani Enterprises", "Bharti Airtel", "ITC"
]

sectors = [
    "Energy", "IT", "Banking", "Telecom", "FMCG",
    "Infrastructure", "Finance"
]

market_events = [
    "Positive Earnings", "Interest Rate Hike", "Market Rally",
    "Inflation Concern", "Geopolitical Tension",
    "Regulatory Change", "Economic Slowdown"
]

print("Stock master data created successfully.")

[ ]
# CELL 3: Generate synthetic market dataset

n = 150

df = pd.DataFrame({
    "Stock_ID": range(3001, 3001 + n),
    "Stock_Name": np.random.choice(stocks, n),
    "Sector": np.random.choice(sectors, n),
    "Current_Price": np.random.randint(100, 5000, n),
    "Daily_Return_Percent": np.round(np.random.uniform(-8, 8, n), 2),
    "Volatility_Index": np.round(np.random.uniform(0.1, 1.0, n), 2),
    "Trading_Volume": np.random.randint(10000, 5000000, n),
    "Market_Event": np.random.choice(market_events, n),
    "Interest_Rate_Percent": np.round(np.random.uniform(4, 9, n), 2),
    "Inflation_Rate_Percent": np.round(np.random.uniform(3, 9, n), 2)
})

df.head(10)

[ ]
# CELL 4: Create market risk classification logic

market_risk = []
investment_action = []
risk_reason = []
portfolio_strategy = []

for index, row in df.iterrows():

    volatility = row["Volatility_Index"]
    daily_return = abs(row["Daily_Return_Percent"])
    inflation = row["Inflation_Rate_Percent"]

    score = 0
    reasons = []

    # High volatility
    if volatility > 0.75:
        score += 0.4
        reasons.append("High market volatility")

    # Large daily movement
    if daily_return > 5:
        score += 0.3
        reasons.append("Large daily price fluctuation")

    # High inflation
    if inflation > 7:
        score += 0.2
        reasons.append("High inflation environment")

    # Negative macro events
    if row["Market_Event"] in ["Economic Slowdown", "Geopolitical Tension", "Interest Rate Hike"]:
        score += 0.2
        reasons.append("Negative market event")

    # Risk classification
    if score >= 0.7:
        market_risk.append("High")
        investment_action.append("Reduce Exposure / Hedge")
        portfolio_strategy.append("Use defensive assets or diversification")

    elif score >= 0.4:
        market_risk.append("Medium")
        investment_action.append("Hold / Monitor Carefully")
        portfolio_strategy.append("Balanced portfolio management recommended")

    else:
        market_risk.append("Low")
        investment_action.append("Buy / Long-Term Hold")
        portfolio_strategy.append("Suitable for stable long-term investment")

    risk_reason.append(", ".join(reasons) if reasons else "Stable market behavior")

df["Market_Risk_Level"] = market_risk
df["Investment_Action"] = investment_action
df["Risk_Reason"] = risk_reason
df["Portfolio_Strategy"] = portfolio_strategy

df.head(10)

[ ]
# CELL 5: View complete market risk profile

selected_columns = [
    "Stock_ID", "Stock_Name", "Sector", "Current_Price",
    "Daily_Return_Percent", "Volatility_Index",
    "Trading_Volume", "Market_Event",
    "Interest_Rate_Percent", "Inflation_Rate_Percent",
    "Market_Risk_Level", "Investment_Action",
    "Risk_Reason", "Portfolio_Strategy"
]

df[selected_columns].head(10)

[ ]
# CELL 6: Market risk distribution

risk_distribution = df["Market_Risk_Level"].value_counts()

print("Market Risk Distribution:")
print(risk_distribution)

[ ]
# CELL 7: Visualize market risk distribution

plt.figure(figsize=(7,5))
sns.countplot(data=df, x="Market_Risk_Level", order=["Low", "Medium", "High"])
plt.title("Distribution of Market Risk Levels")
plt.xlabel("Market Risk Level")
plt.ylabel("Number of Stocks")
plt.show()

[ ]
# CELL 8: Analyze volatility by market risk level

plt.figure(figsize=(8,5))
sns.boxplot(data=df, x="Market_Risk_Level", y="Volatility_Index",
            order=["Low", "Medium", "High"])
plt.title("Volatility by Market Risk Level")
plt.xlabel("Market Risk Level")
plt.ylabel("Volatility Index")
plt.show()

[ ]
# CELL 9: Analyze daily return fluctuations

plt.figure(figsize=(8,5))
sns.boxplot(data=df, x="Market_Risk_Level", y="Daily_Return_Percent",
            order=["Low", "Medium", "High"])
plt.title("Daily Return Percentage by Market Risk")
plt.xlabel("Market Risk Level")
plt.ylabel("Daily Return %")
plt.show()

[ ]
# CELL 10: Sector-wise market risk analysis

sector_summary = df.groupby("Sector").agg(
    Average_Volatility=("Volatility_Index", "mean"),
    Average_Return=("Daily_Return_Percent", "mean"),
    Average_Inflation_Impact=("Inflation_Rate_Percent", "mean")
).round(2)

sector_summary.sort_values(by="Average_Volatility", ascending=False)

[ ]
# CELL 11: Identify high market risk stocks

high_risk_stocks = df[df["Market_Risk_Level"] == "High"]

high_risk_stocks[selected_columns].head(10)

[ ]
# CELL 12: Identify low market risk stocks

low_risk_stocks = df[df["Market_Risk_Level"] == "Low"]

low_risk_stocks[selected_columns].head(10)

[ ]
# CELL 13: Create stock-level explanation function

def explain_market_risk(stock_id):

    stock = df[df["Stock_ID"] == stock_id]

    if stock.empty:
        print("Stock ID not found.")
        return

    row = stock.iloc[0]

    print("Market Risk Analysis")
    print("--------------------")
    print(f"Stock Name: {row['Stock_Name']}")
    print(f"Sector: {row['Sector']}")
    print(f"Current Price: ₹{row['Current_Price']}")
    print(f"Daily Return %: {row['Daily_Return_Percent']}")
    print(f"Volatility Index: {row['Volatility_Index']}")
    print(f"Trading Volume: {row['Trading_Volume']}")
    print(f"Market Event: {row['Market_Event']}")
    print(f"Interest Rate %: {row['Interest_Rate_Percent']}")
    print(f"Inflation Rate %: {row['Inflation_Rate_Percent']}")
    print(f"Market Risk Level: {row['Market_Risk_Level']}")
    print(f"Investment Action: {row['Investment_Action']}")
    print(f"Risk Reason: {row['Risk_Reason']}")
    print(f"Portfolio Strategy: {row['Portfolio_Strategy']}")

# Example
explain_market_risk(3001)

[ ]
# CELL 14: Export market risk dataset

df.to_csv("synthetic_market_risk_dataset.csv", index=False)

print("Market risk dataset exported successfully.")

[ ]
# CELL 15: Download dataset in Google Colab

try:
    from google.colab import files
    files.download("synthetic_market_risk_dataset.csv")
except ImportError:
    print("Download works only in Google Colab.")
    print("Dataset saved locally.")

[ ]
# CELL 16: Create market risk summary report

summary_report = df.groupby("Market_Risk_Level").agg(
    Total_Stocks=("Stock_ID", "count"),
    Average_Volatility=("Volatility_Index", "mean"),
    Average_Return=("Daily_Return_Percent", "mean"),
    Average_Inflation=("Inflation_Rate_Percent", "mean")
).round(2)

summary_report.to_csv("market_risk_summary_report.csv")

summary_report

[ ]
# CELL 17: Final outcomes and industry relevance

print("LAB OUTCOMES")
print("------------")
print("1. Students created a synthetic stock market dataset.")
print("2. Students understood market risk indicators such as volatility and daily return.")
print("3. Students classified stocks into Low, Medium, and High market risk.")
print("4. Students analyzed the effect of macroeconomic events on investments.")
print("5. Students visualized market risk patterns using analytics.")
print("6. Students understood how AI supports portfolio risk management.")

print("\nREAL-WORLD INDUSTRY RELEVANCE")
print("-----------------------------")
print("Investment banks, hedge funds, and portfolio managers use similar market risk analytics")
print("to manage investments, reduce losses, and optimize portfolio strategies.")
Student Mini Assignment
Students should:

Identify 5 high market risk stocks and explain why they are risky.
Identify 5 low market risk stocks and explain why they are safer.
Explain how volatility affects investment decisions.
Explain why inflation and interest rates impact stock markets.
Suggest one improvement to make the market risk model more realistic.
Suggested GitHub Folder Structure
Market-Risk-Analytics-Lab/
│
├── README.md
├── notebook/
│   └── Market_Risk_Analytics_Lab.ipynb
├── dataset/
│   └── synthetic_market_risk_dataset.csv
├── screenshots/
│   ├── market_risk_distribution.png
│   ├── volatility_analysis.png
│   └── return_analysis.png
└── report/
    └── market_risk_report.pdf
