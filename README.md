# MSCS 634 — Project Deliverable 2

## Regression Modeling and Performance Evaluation
### Factors Associated with Five-Year Home Price Growth in California

**Name:** Chun-Shuo Sheu
**Course:** MSCS 634 — Big Data and Data Mining
**Deliverable:** 2 of 4 — Regression Modeling and Performance Evaluation

---

## The two questions

1. **Which factors best predict five-year home price growth?**
2. **Given current conditions, what will homes be worth in five years (2029)?**

## Reproducing

```bash
pip install -r requirements.txt

# 1. D1's cleaned panel must be present (copied in, or rebuilt via D1's scripts):
#      data/processed/ca_zip_housing_panel_clean.csv
# 2. Raw Zillow/FRED/ACS files in data/raw/ -- see D1's README for download instructions.
#    Only needed to rebuild the 2024 forecast frame.

python scripts/build_forecast_frame.py
python scripts/make_deliverable_2_notebook.py
jupyter nbconvert --execute --inplace notebooks/Project_Deliverable_2.ipynb
```

## Summary

If we look at the data, California theoretically has close to 2,000 ZIP Codes. If five base years are observed, the original dataset could contain nearly 10,000 records. However, after integrating data from multiple sources—including Zillow home prices, population, income, rent, unemployment rates, and mortgage rates—and removing records that could not be fully matched or did not meet the analytical requirements, the first Deliverable produced a final dataset containing 7,487 observations and 28 variables. Each observation represents the market and socioeconomic conditions of a California ZIP Code in a specific base year, along with the actual home-price growth rate in that area over the following five years.

In the first Deliverable, we primarily conducted exploratory data analysis to examine the relationships between different variables, current home prices, and future home-price growth. The results showed that variables such as the price-to-income ratio, price-to-rent ratio, population, household income, rent, educational attainment, and unemployment rate had varying degrees of association with housing market performance. However, the correlation of any individual variable can only indicate whether two variables move together. It cannot prove that one factor directly causes home prices to increase or decrease, nor can it independently determine the predictive power of multiple variables when they are considered simultaneously.

Therefore, in the second Deliverable, I further applied regression models to examine whether these features could jointly predict home-price growth over the following five years. The data were divided chronologically, with 2016–2018 used as the training data, 2019 used as the validation data, and 2020 used as the final out-of-time test data. The models included a baseline model that predicted using the mean of the training data, Ridge Regression, Random Forest Regression, and Gradient Boosting Regression. This time-based split more closely reflects a real-world forecasting scenario than a random split because the purpose of the study is to use data from earlier years to predict home-price changes in later years, rather than allowing the model to observe similar market conditions from overlapping periods in both the training and testing datasets.

The models performed better than the baseline in both cross-validation and the 2019 validation data, indicating that ZIP Code-level population, income, home-price, and affordability features contained some useful predictive information. However, when the models were used to predict five-year growth following the 2020 base year, all models produced negative test R² values. This indicates that the models could not reliably predict the absolute level of home-price growth across California. The main issue was not that all features lacked value, but that the overall housing market environment differed substantially across the observed periods. The average five-year growth rate was approximately 47.96% in the training data, 38.15% in the validation data, and only 24.31% in the test data. Changes in mortgage rates, macroeconomic conditions, market supply and demand, and the housing market cycle caused models trained on earlier years to overestimate the overall level of growth in later periods.

To distinguish statewide market movements from the characteristics of individual ZIP Codes, the second-stage analysis subtracted the statewide median ZIP Code growth rate for each base year from the five-year growth rate of each ZIP Code. The revised target was no longer to predict “how much the overall California housing market would increase,” but rather to predict “how much better or worse a particular ZIP Code would perform relative to other ZIP Codes in the same period.” After this adjustment, Random Forest and Gradient Boosting achieved positive test R² values of approximately 0.12, while the rank correlation coefficients were approximately 0.4. Although this predictive power remained limited, the results suggest that ZIP Code-level characteristics can help identify areas that are likely to experience relatively higher or lower performance.

The analysis showed that affordability and population were among the more stable predictive factors. Areas in which home prices were high relative to local income or rent generally experienced lower relative growth over the following five years. In contrast, areas where housing remained relatively affordable may have had greater room for future appreciation. In the predicted rankings, several ZIP Codes in Kern County, Lake County, the San Bernardino desert area, and the Central Valley were identified as areas with relatively higher expected growth, while several higher-priced ZIP Codes in Los Angeles were ranked as areas with relatively lower expected growth.

Therefore, the most important conclusion of this study is not that the models can precisely predict California home prices over the next five years. Instead, the results indicate that the absolute level of statewide home-price growth is largely influenced by macroeconomic conditions and the prevailing market regime. ZIP Code-level features alone are insufficient for producing reliable forecasts of the overall market. Nevertheless, after controlling for the statewide market level in each year, ZIP Code characteristics such as population and housing affordability retain some relative predictive power. These features can therefore be used to compare which areas may outperform or underperform the broader market during the same period. In other words, these models are better suited for relative ranking among ZIP Codes than for producing precise statewide home-price forecasts.

