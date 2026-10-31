# CardioGood Fitness – Customer Profiling & Data Analysis

Descriptive analysis of 180 treadmill buyers to build a customer profile for each CardioGood Fitness product line (TM195, TM498, TM798), plus a simple regression predicting weekly miles.

## Dataset
`CardioGoodFitness.csv` – 180 customers, 9 columns: Product, Age, Gender, Education, MaritalStatus, Usage, Fitness, Income, Miles.

## What's inside
- Data overview and univariate analysis (histograms, boxplots)
- Bivariate analysis: product vs. gender and marital status (crosstabs, pivot tables, count plots)
- Correlation analysis (heatmap, pairplot)
- Linear regression: `Miles ~ Usage + Fitness`
- Conclusions and customer profiles

## Key findings
| Product | Customers | Avg age | Avg income | Avg usage/wk | Avg fitness | Avg miles/wk | Female / Male |
|---|---|---|---|---|---|---|---|
| TM195 | 80 | 28.6 | $46,418 | 3.1 | 3.0 | 82.8 | 50% / 50% |
| TM498 | 60 | 28.9 | $48,974 | 3.1 | 2.9 | 87.9 | 48% / 52% |
| TM798 | 40 | 29.1 | $75,442 | 4.8 | 4.6 | 166.9 | 18% / 82% |

- **TM798** is the premium product: mostly male, higher income and education, very fit, and about twice the weekly miles of the other models.
- **TM195 / TM498** attract very similar, more casual customers, split evenly by gender.
- Marital status (~60% partnered) does not noticeably affect product choice.
- Regression: `Miles ≈ -56.74 + 20.21·Usage + 27.21·Fitness` (R² ≈ 0.72).

## Tools
Python, pandas, NumPy, seaborn, matplotlib, scikit-learn, Jupyter.

## How to run
```bash
git clone https://github.com/Bilal-Butt/CardioGood-Fitness-Data-Analysis.git
cd CardioGood-Fitness-Data-Analysis
pip install -r requirements.txt
jupyter notebook CardioGood_Fitness_Customer_Profiling.ipynb
```
Run all cells; outputs are cleared in the repo and regenerate on execution.

## Notes
This project was completed as a case study in AI-assisted coding and data analysis. Findings are descriptive (small sample, three-month window), not causal.
