
# Market Sentiment and Trader Performance Analysis Report

**Candidate Name:** Sangeetha Panneerselvam

## Introduction

This report analyzes the relationship between market sentiment, as measured by the Fear and Greed Index, and trader performance, using historical trading data. The objective is to uncover hidden patterns and provide insights for smarter trading strategies.

## Data Overview

The analysis utilized two datasets:

- `fear_greed_df`: Contains historical Fear and Greed Index values and classifications.
- `historical_df`: Contains detailed historical trading data, including execution prices, trade sizes, timestamps, and closed PnL.

Both datasets were checked for missing values and none were found. Date and timestamp columns were converted to datetime objects for time-series analysis and merging.

## Data Cleaning and Preprocessing

- No missing values were found in either dataset.
- Date and timestamp columns (`date`, `timestamp` in `fear_greed_df`, and `Timestamp IST`, `Timestamp` in `historical_df`) were successfully converted to datetime objects.
- Box plots of numerical columns in `historical_df` (`Execution Price`, `Size Tokens`, `Size USD`, `Start Position`, `Closed PnL`, `Fee`) revealed the presence of potential outliers, which is common in financial data. For this analysis, outliers were not explicitly removed, but their presence was noted.

## Feature Engineering

- A 'date' column was extracted from the `Timestamp IST` in `historical_df` to facilitate merging with `fear_greed_df`.
- Daily performance metrics, including `total_closed_pnl` and `trade_count` per account per day, were calculated from `historical_df`.
- Lagged features for the Fear and Greed Index value and classification (1, 3, and 7 days) were created in `fear_greed_df`.

## Data Merging

The `daily_performance_df` (containing aggregated daily trader performance) was merged with `fear_greed_df` on the 'date' column using a left merge. This combined market sentiment data with daily trader performance metrics, creating the `merged_df`.

## Exploratory Data Analysis (EDA)

- The distribution of the Fear and Greed Index values showed a spread across the sentiment spectrum, with concentrations in the "Fear" and "Extreme Greed" categories.
- The distribution of `total_closed_pnl` was heavily skewed, with a large number of observations around zero and some significant positive and negative values, indicating the presence of profitable and unprofitable trading days.
- A scatter plot between Fear and Greed Index value and `total_closed_pnl` did not reveal a strong linear correlation.
- Box plots of `total_closed_pnl` across different sentiment classifications showed variations in the distribution of PnL, but no single sentiment category consistently demonstrated significantly higher or lower average profitability compared to others.
- The correlation matrix confirmed a very low correlation (0.000179) between the Fear and Greed Index value and `total_closed_pnl`. However, there was a moderate positive correlation (0.175620) between `total_closed_pnl` and `trade_count`, suggesting that higher trading volume might be associated with higher PnL (though not necessarily indicating causality). The lagged sentiment values showed high correlation with the current sentiment value, as expected.

## Statistical Analysis

- **ANOVA for Total Closed PnL:** An ANOVA test was conducted to compare the average `total_closed_pnl` across different Fear and Greed Index classifications. The results showed no statistically significant difference (p-value = 0.657 > 0.05). This suggests that, based on this dataset, the Fear and Greed Index classification alone does not significantly impact the average daily trading profitability.
- **ANOVA for Trade Count:** An ANOVA test on `trade_count` across sentiment classifications revealed a statistically significant difference (p-value = 0.012 < 0.05). This indicates that market sentiment, as categorized by the index, does influence the number of trades executed by traders.
- **Linear Regression Model:** A linear regression model was built to predict `total_closed_pnl` using the current Fear and Greed Index value and its lagged values (1, 3, and 7 days). The model had a very low R-squared value (0.003), indicating that these sentiment variables explain a negligible amount of the variance in `total_closed_pnl`. Only the 7-day lagged value showed a statistically significant coefficient (p-value = 0.037), but the overall model's predictive power is minimal.

## Insights and Recommendations

Based on the analysis of the provided datasets:

- **The Fear and Greed Index is not a direct predictor of average trading profitability.** The statistical analysis showed no significant relationship between sentiment classification or value and the average daily Total Closed PnL. Traders should not rely solely on this index as a primary signal for market entry or exit based on expected profit or loss.
- **Market sentiment influences trading behavior (volume).** The significant difference in trade counts across sentiment classifications suggests that traders are more or less active depending on the prevailing market sentiment. While this doesn't directly translate to higher PnL based on this analysis, increased trading activity can lead to higher transaction costs (fees) or potential slippage, which could indirectly impact overall profitability.
- **Other factors are likely the primary drivers of trading performance.** The low explanatory power of the sentiment variables in predicting PnL indicates that successful trading in this dataset is more likely influenced by other factors not included in this analysis. These could include specific trading strategies, risk management techniques, asset selection, timing relative to other market events, or individual trader skill.

**Recommendations for Smarter Trading Strategies:**

1. **Diversify your analysis beyond sentiment:** While sentiment provides context, focus on incorporating other technical, fundamental, or quantitative analysis methods that have a more direct impact on price movements and trading opportunities.
2. **Be mindful of trading costs during periods of high sentiment-driven activity:** Since sentiment influences trade volume, be aware that increased trading during extreme sentiment periods might lead to higher accumulated fees or less favorable execution prices. Consider optimizing trade execution during such times.
3. **Focus on robust trading strategies and risk management:** The analysis reinforces the importance of having well-defined trading strategies and strict risk management protocols, as these are likely more significant determinants of profitability than market sentiment alone.
4. **Consider sentiment as a behavioral indicator:** Use the Fear and Greed Index as a tool to understand the prevailing market psychology and potential shifts in overall trading activity, rather than a direct signal for price direction or profitability.

## Conclusion

The analysis of the provided datasets indicates that while market sentiment, as measured by the Fear and Greed Index, influences trading volume, it does not show a statistically significant direct relationship with average trading profitability. Successful trading in this context appears to be driven by factors other than this sentiment index. Future analysis could explore these other factors in more detail to identify more effective trading strategies.
