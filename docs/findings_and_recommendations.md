# Findings and Recommendations

Companion to the [notebook](../notebooks/cardiogood_fitness_customer_profiling.ipynb). Dataset: 180 treadmill buyers (TM195: 80, TM498: 60, TM798: 40). All tests use alpha = 0.05.

---

## 1. Customer profile by product

| | TM195 | TM498 | TM798 |
|---|---:|---:|---:|
| Customers | 80 (44%) | 60 (33%) | 40 (22%) |
| Average age | 28.6 | 28.9 | 29.1 |
| Female share | 50.0% | 48.3% | 17.5% |
| Partnered share | 60.0% | 60.0% | 57.5% |
| Education (years) | 15.0 | 15.1 | 17.3 |
| Average income | $46,418 | $48,974 | $75,442 |
| Planned usage per week | 3.1 | 3.1 | 4.8 |
| Self-rated fitness (1-5) | 3.0 | 2.9 | 4.6 |
| Expected miles per week (mean / median) | 82.8 / 85 | 87.9 / 85 | 166.9 / 160 |

- **TM195 (entry-level):** a casual customer around 29, men and women equally, income about $46k, planning about three sessions a week at average fitness.
- **TM498 (mid-range):** almost the same customer as TM195 (income about $49k, same usage, fitness and miles).
- **TM798 (high-performance):** mostly male (82.5%), better educated, income about $75k, very fit (4.6 of 5), planning nearly five sessions a week and expecting about 167 miles.

## 2. Statistical tests

| Question | Test | Result |
|---|---|---|
| Product vs. gender | Chi-square | chi-square = 12.92, dof = 2, **p = 0.002**, Cramer's V = 0.27 |
| Product vs. marital status | Chi-square | chi-square = 0.08, dof = 2, p = 0.96, Cramer's V = 0.02 |
| Age across products | One-way ANOVA | F = 0.09, p = 0.91 |
| Education across products | One-way ANOVA | F = 45.2, p < 0.001 |
| Income across products | One-way ANOVA | F = 89.3, p < 0.001 |
| Usage across products | One-way ANOVA | F = 65.4, p < 0.001 |
| Fitness across products | One-way ANOVA | F = 104.0, p < 0.001 |
| Miles across products | One-way ANOVA | F = 68.2, p < 0.001 |

Pairwise (Tukey HSD) comparisons for income, miles, usage and fitness: **TM798 differs from both other products (all p < 0.001); TM195 vs. TM498 does not differ** (p = 0.41 income, 0.72 miles, 0.99 usage, 0.84 fitness).

Other facts worth knowing:
- All 23 customers earning above $70k bought a TM798, as did all 21 above $75k.
- 29 of the 31 customers rating their fitness 5 bought a TM798.
- Men have a higher average income overall ($56.6k vs $49.8k), but within each product men and women earn about the same, so the gap reflects the product mix (more men buy the high-income TM798).

## 3. Correlations

| Pair | Correlation |
|---|---:|
| Fitness - Miles | 0.79 |
| Usage - Miles | 0.76 |
| Usage - Fitness | 0.67 |
| Education - Income | 0.63 |
| Income - Miles | 0.54 |
| Fitness - Income | 0.54 |
| Age - Income | 0.51 |
| Age - Usage / Fitness / Miles | 0.02 to 0.06 |

## 4. Regression: expected weekly miles

`Miles = -56.74 + 20.21 × Usage + 27.21 × Fitness`

- Holding Fitness fixed, each extra planned weekly session goes with about 20 more expected miles; holding Usage fixed, each extra fitness point goes with about 27 more miles. Both coefficients are significant (p < 0.001).
- In-sample R² = 0.716 (RMSE about 28 miles per week).
- 5-fold cross-validated R² averages 0.61 (range 0.26 to 0.82), so with 180 customers the fit is less stable than the in-sample figure suggests.
- Usage and Fitness are correlated (0.67), so individual coefficients should be read with care. The negative intercept has no practical meaning because no customer plans zero usage.

## 5. Answers to the guiding questions

1. **Variable types:** six numeric (Usage and Fitness are ordinal scores) and three categorical; no missing values.
2. **Distributions:** Age, Income and Miles are right-skewed; most customers are in their 20s, plan 3-4 sessions a week and rate their fitness 3.
3. **Product by gender:** TM195 and TM498 are bought almost equally by men and women; TM798 is bought mostly by men (33 of 40).
4. **Marital status:** does not affect product choice (p = 0.96).
5. **Correlations:** strong for Fitness/Usage with Miles; Age is nearly uncorrelated with usage.
6. **Miles by product:** TM195 and TM498 have a median of 85 per week each; TM798 has a median of 160 and the widest spread (up to 360).

## 6. Recommendations

1. **Position the TM798 as the premium, performance product** for higher-income (about $75k+), highly fit customers who plan to train around five times a week.
2. **Investigate the gender gap on TM798.** Only 18% of its buyers are women, versus about half for the other models. The data cannot say why (marketing reach, features or preference), so test it with targeted campaigns and customer surveys before drawing conclusions.
3. **Treat TM195 and TM498 as one casual segment.** Their buyers are very similar on every measured attribute and statistically indistinguishable on income, usage, fitness and miles. Make the difference between the two clear through features or price, or consider simplifying the line-up. This dataset has no price or feature information, so this needs to be tested.
4. **Do not segment by age or marital status.** Neither separates the products.
5. **Guide buyers at the point of sale** with two questions, planned weekly usage and fitness level. They are the strongest signals of which tier a customer ends up buying.
6. **Collect richer data:** non-buyers, price paid, satisfaction and repeat purchases would allow modelling of who buys, not just describing who bought.

## 7. Limitations

- Small sample: 180 customers from a three-month window at CardioGood Fitness retail stores; some segments are tiny (7 women bought a TM798).
- Buyers only: the profiles cannot show what drives a purchase or describe the wider market.
- Self-reported and forward-looking: Usage, Miles and Fitness are expectations and ratings, not measured behaviour.
- Overlapping drivers: Income, Education, Fitness and Usage are correlated.
- Association, not causation. Outliers in Miles (above 200, mostly TM798) were kept.
