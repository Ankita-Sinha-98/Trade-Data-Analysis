# Trade-Data-Analysis
Analysis historical trade data from Binance, to gain account insights based on data metrics

![Final score SS](https://github.com/user-attachments/assets/7c12c2be-aed2-4938-b3c3-031ee02db508)

Metrics Considered:
ROI (Return on Investment)
PnL (Profit and Loss)
Sharpe Ratio
MDD (Maximum Drawdown)
Win Rate
Win Positions
Total Positions

I have used Jupyter Notebook with Python to solve this problem. The final python script on the Notebook is attached for your reference, and these are the details of my Data Analysis on the dataset provided:

Step 1: Data Exploration and Cleaning
  
  1. Load the Dataset:
    ○ I have used pandas library to load the CSV file into a
      DataFrame.
    ○ Check for missing values and handle them appropriately
      (e.g., I have removed the null values and duplicate rows in order to clean data).
    ○ Inspected the first few rows (using head()) to understand the structure and types of data.
  2. Convert Data Types:
    ○ Ensured timestamps are in datetime format.
    ○ Converted the “Trade History” JSON format into rows that we can analyse and use and manipulate using the ast package and
      json_normalise() functions.
  3. Handle Duplicates & Anomalies:
    ○ Checkedforduplicateentriesandremoveifnecessary.
    ○ Alsoremovednullvalueswhichwerenotrecognised.


Step 2: Feature Engineering

  1. Classify Positions:
    ○ Combined side (BUY/SELL) and positionSide to determine whether a trade opens or closes a position.
    ○ Example categories: long_open, long_close, short_open, short_close.
  2. Calculate Key Metrics Per Account:
    ○ PnL(ProfitandLoss):
          PnL =∑realizedProfit
    ○ ROI(ReturnonInvestment):
           ROI = (PnL/InitialInvestment)×100
      Estimated initial investment by summing quantity of first trades.
    ○ WinRate:
      Win Rate = (Win Positions/Total Positions)×100 A win position is when realizedProfit > 0.
    ○ SharpeRatio:
      Sharpe Ratio = Mean (Daily Returns) / Std Dev (Daily Returns)
       Calculate daily returns based on closing trades.
    ○ MDD(MaximumDrawdown):
      Track peak portfolio value and compute largest percentage drop.


Step 3: Ranking Algorithm

NOTE: In the first code, I had included Sharpe Ratio after ROI and PnL metrics, but after further calculation I saw that the Sharpe Ratio was 0.0 for all Port_IDs, which means that the fund's returns are exactly equal to the returns of a risk-free asset, indicating that the fund is not providing any additional return for the risk it takes on.
So, I skipped including the Sharpe Ratio in my Final Score Calculation and used the rest of the metrics:
  
  Normalization Technique
● Min-MaxNormalization
● Scalesallmetricsto0-1range
● Preservesrelativeperformancedifferences
   
   Scoring Formula
Final Score = ( Normalized_ROI * 0.35 + Normalized_PnL * 0.3 + Normalized_Win_Rate * 0.25 + (1 - Normalized_MDD) * 0.1 )
   
   Weighting Rationale
● ROI:35%(Mostcriticalperformanceindicator) ● PnL:30%(Absoluteprofitmeasurement)
● WinRate:25%(Consistencyindicator)
● MDD:10%(Riskmanagementfactor)
   
   Key Ranking Principles
● Positivescoresindicatebetterperformance
● Centeredaroundaverageperformance
● Allowscomparisonacrossdifferenttradingaccounts
   
   Limitations and Challenges
1. Data Dependent Limitations
  ○ Performance heavily relies on quality and completeness of
  input data
  ○ Shorttradingperiodsmightskewresults
  ○ Therewereoutliersandmissingvaluesallofwhomhadtobe
  dealt with
2. Metric Calculation Constraints
  ○ Assumeslinearrelationshipbetweenmetrics
  ○ Doesnotaccountforcomplextradingstrategies ○ Staticweightsmightnotsuitalltradingstyles
3. Risk Assessment
  ○ SimpleMDDcalculationmightnotcapturenuancedrisk ○ Doesnotincorporatemarketvolatility
  ○ Ignoresexternalmarketconditions
4. Computational Limitations
  ○ Requirescompletetradehistory
  ○ Sensitivetooutliers
  ○ Assumesconsistenttradingbehavior
5. Performance Ranking Caveats
○ Rankingsarerelativewithinthedataset
○ Pastperformancedoesnotguaranteefutureresults
○ Doesnotpredictfuturetradingsuccess

   Potential Improvements
● Implementmachinelearningmodels
● Incorporatemoresophisticatedriskmetrics ● Dynamicweightadjustment
● Time-seriesbasedanalysis
● Advanceddrawdowncalculations


Explanation of the Ranking System
The ranking system is designed to evaluate trading strategies or portfolios based on five key performance metrics. Each metric is weighted differently based on its importance. The Final Score is a weighted sum of these metrics, where higher scores indicate better performance.

Breakdown of Metrics and Weights
1. Return on Investment (ROI) – 35% weight
○ Measuresthepercentagegainorlossrelativetotheinitial
investment.
○ Higher is better because a higher ROI means greater
           profitability.
○ Weight: 0.35 → This gets the highest weight since
profitability is a key goal.

2. Sharpe Ratio – 0% weight
○ (Explainedabove)

3. Profit and Loss (PnL) – 30% weight
○ Representstheabsoluteprofitorlossinmonetaryterms.
○ Higher is better because a larger PnL means more money earned.
○ Weight: 0.30 → Important but slightly less than ROI since absolute profit depends on account size.

4. Win Rate – 25% weight
○ Percentageoftradesthatwereprofitable.
○ Higher is better because more winning trades indicate
           consistency.
○ Weight: 0.25 → Less weight because a high win rate doesn’t
           always mean high profitability.

5. Maximum Drawdown (MDD) – (-10%) weight
○ Measuresthelargestdropincapitalfrompeaktotrough.
○ Lowerisbetter(negativeimpact),sowemultiplyby-0.1to
           penalize higher drawdowns.
○ Weight: -0.1 → It has the least weight but still matters
           since large drawdowns indicate risk.
