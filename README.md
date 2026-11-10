# Billionaire Insights

An R Markdown analysis of a 2023 dataset of 2,640 billionaires: cleaning, missing data handling, EDA and two non-parametric tests on net worth.

## Questions

- Which countries and industries have the most billionaires?
- Do self-made billionaires have a different net worth than those who inherited their wealth?
- Does net worth differ across industries?

## Data

[Billionaires Statistics Dataset](https://www.kaggle.com/datasets/nelgiriyewithana/billionaires-statistics-dataset) from Kaggle: 2,640 billionaires and 35 variables. `finalWorth` (net worth in millions of USD) is the main variable. Other columns include age, gender, self-made status, industry, country and city, plus country-level indicators such as GDP, population, life expectancy and tax rates.

The CSV is not included in this repo. Download it from Kaggle.

## Methods

- Dropped 15 columns I did not need (birth date parts, names, collection date, state, CPI, education enrollment, coordinates).
- Kept the top 25 cities, countries and citizenships and grouped the rest as "Other". Converted categorical columns to factors and cleaned the `gdp_country` text into numbers.
- EDA with ggplot2: billionaires per country and per industry, and the net worth distribution on a log scale.
- Checked missingness with naniar and `mice::md.pattern`. Multiple imputation with mice did not work because several numeric country-level variables are almost perfectly correlated, so I used median imputation and compared the distributions before and after.
- Added `wealth_per_age` and `log_finalWorth`.
- Net worth is strongly right-skewed even after a log transform, so I used non-parametric tests: Wilcoxon rank-sum for self-made vs. not, and Kruskal-Wallis across the 10 largest industries.

## Results

From the rendered report:

- Self-made: 1,812 billionaires, median net worth $2.2B, mean $4.47B. Not self-made: 828, median $2.6B, mean $4.97B.
- Wilcoxon rank-sum test: W = 809577, p = 0.001. The net worth distributions of the two groups differ.
- Kruskal-Wallis test across the top 10 industries: χ² = 30.655, df = 9, p = 0.00034.
- The US and China have by far the most billionaires. Finance & Investments, Manufacturing and Technology are the industries with the most billionaires.

## How to run

R packages: dplyr, tidyr, ggplot2, naniar, mice, corrplot, grid.

Download the CSV from Kaggle, change the `read.csv()` path at the top of `billionaries.Rmd`, and knit it.

## Files

- `billionaries.Rmd`: the analysis
- `Billionaire Insights.html`: the rendered report with all plots and outputs

## Limitations

- The data is one snapshot (April 2023).
- Median imputation shrinks the spread of the imputed variables.
- The analysis stops at the hypothesis tests. No predictive model of net worth is fitted.
