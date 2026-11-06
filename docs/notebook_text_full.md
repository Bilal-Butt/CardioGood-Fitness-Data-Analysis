# Full Notebook Narrative

All markdown text from [`cardiogood_fitness_customer_profiling.ipynb`](../notebooks/cardiogood_fitness_customer_profiling.ipynb) in order: problem statement, data dictionary, observations after each chart and test, answers to the guiding questions, recommendations and limitations. Code is omitted; see the notebook.

---

# Problem Statement :   CardioGood Fitness Data Analysis

## Context
 The market research team at AdRight is assigned the task to identify the profile of the typical customer for each treadmill product offered by CardioGood Fitness. The market research team decides to investigate whether there are differences across the product lines with respect to customer characteristics. The team decides to collect data on individuals who purchased a treadmill at a CardioGood Fitness retail store at any time in the past three months. The data is stored in the CardioGoodFitness.csv file.

## Objective
 Perform descriptive analysis to create a customer profile for each CardioGood Fitness treadmill product line.

## Data Dictionary

**The team identified the following customer variables to study:**

* Product: Product purchased - TM195, TM498, or TM798
* Gender: Male or Female
* Age: Age of the customer in years
* Education: Education of the customer in years
* MaritalStatus: Single or partnered
* Income: Annual household income
* Usage: The average number of times the customer plans to use the treadmill each week;
* Miles: The average number of miles the customer expects to walk/run each week;
* Fitness: Self-rated fitness on a 1-to-5 scale, where 1 is the poor shape and 5 is excellent shape.

## Questions to explore
* What are the different types of variables in the data?
* What is the distribution of different variables in the data?
* Which product is more popular among males or females?
* Is the product purchase affected by the marital status of the customer?
* Is there a significant correlation among some of the variables?
* What is the distribution of the average number of miles for each product?

**Data overview.** 180 customers and 9 columns, with **no missing values and no duplicate rows**. Six variables are numeric (Age, Education, Usage, Fitness, Income, Miles; Usage and Fitness are ordinal scores) and three are categorical (Product, Gender, MaritalStatus). TM195 is the most purchased product (80 customers, 44%), followed by TM498 (60, 33%) and TM798 (40, 22%).

**Distributions.** Age, Income and Miles are right-skewed: most customers are in their 20s (mean 28.8, median 26, range 18-50), income has a mean of $53.7k vs a median of $50.6k (maximum $104.6k), and weekly miles has a median of 94 with a long tail up to 360. Most customers plan to use the treadmill 3-4 times a week (121 of 180) and rate their fitness as 3 (97 of 180).

**Age by gender and product.** Age distributions are very similar for men and women (mean 29.0 vs 28.6), and across the three products (product means 28.6, 28.9 and 29.1 years). Age does not distinguish the product lines (one-way ANOVA p = 0.91, shown later).

**Product by gender.** TM195 is split evenly (40 women, 40 men) and TM498 almost evenly (29 / 31), but TM798 is strongly male-dominated (7 women, 33 men: 82.5% male). A chi-square test confirms the association (p = 0.002, shown later).

**Segment pivots (Product x Gender x Marital status).** TM798 buyers have the highest mean income (about $74-76k vs $46-49k) and the highest weekly miles in every gender/marital group. Within each product, men and women earn similar incomes, so the overall male-female income gap ($56.6k vs $49.8k) mostly reflects more men buying the higher-income TM798 rather than a gap within products. Some cells are very small (for example only 4 partnered and 3 single women bought TM798), so single-cell averages should be read cautiously.

**Pairplot.** Miles rises with Usage and Fitness, and Income rises with Education and Age. Age shows almost no relationship with Usage, Fitness or Miles. The relationships look broadly linear and positive; the plot is not coloured by product, so product differences are examined separately below.

**Age histogram.** Most customers are between 20 and 35, with a long tail to 50 (mean 28.8, standard deviation 6.9).

**Income by gender.** Both distributions are right-skewed. Men have higher incomes on average ($56.6k vs $49.8k) and a higher maximum ($104.6k vs $95.9k). As the pivots above show, this gap is explained by product mix, not by a gap within products.

**Correlations.** The strongest relationships are Fitness-Miles (0.79), Usage-Miles (0.76) and Usage-Fitness (0.67). Education-Income (0.63), Age-Income (0.51) and Income-Miles (0.54) are moderate. Age is essentially uncorrelated with Usage, Fitness and Miles (0.02-0.06). Usage and Fitness are themselves correlated, so their separate effects on Miles overlap. Correlation does not imply causation.

## Customer profile by product

The objective is a profile of the typical buyer of each treadmill. The table below summarises each product, followed by statistical tests of whether the differences are real or could be chance (alpha = 0.05).

**What the tests show.**

- **Gender** is associated with product (chi-square = 12.92, p = 0.002, Cramer's V = 0.27): TM798 skews male.
- **Marital status** is *not* associated with product (chi-square = 0.08, p = 0.96); about 60% of buyers are partnered for every product.
- **Age** does not differ across products (ANOVA p = 0.91).
- **Education, Income, Usage, Fitness and Miles** all differ significantly across products (p < 0.001).
- The pairwise tests show that **TM798 differs from both other products on income, miles, usage and fitness (all p < 0.001), whereas TM195 and TM498 do not differ from each other on any of them** (p between 0.41 and 0.99).

### The typical customer for each product

| | TM195 | TM498 | TM798 |
|---|---|---|---|
| **Share of customers** | 80 (44%) | 60 (33%) | 40 (22%) |
| **Profile** | Casual, entry-level | Casual, mid-range | Serious, high-performance |
| Age | 28.6 | 28.9 | 29.1 |
| Gender | 50% female | 48% female | **18% female (82% male)** |
| Education (years) | 15.0 | 15.1 | **17.3** |
| Income | $46.4k | $49.0k | **$75.4k** |
| Planned usage (per week) | 3.1 | 3.1 | **4.8** |
| Self-rated fitness (1-5) | 3.0 | 2.9 | **4.6** |
| Expected miles per week | 82.8 | 87.9 | **166.9** |

All 23 customers with income above $70k bought a TM798 (as did all 21 above $75k), and 29 of the 31 customers who rate their fitness 5 bought a TM798.

**Regression interpretation.** Holding Fitness fixed, each extra planned weekly session is associated with about **20 more expected miles**, and holding Usage fixed, each extra fitness point with about **27 more miles**. Both coefficients are statistically significant (p < 0.001). The model explains **71.6%** of the variation in miles in-sample (adjusted R-squared about 0.71, typical error about 28 miles per week). On held-out folds the R-squared averages **0.61** and ranges from 0.26 to 0.82, so with only 180 customers the fit is less stable than the in-sample figure suggests. The negative intercept has no practical meaning, because no customer plans zero usage. Usage and Fitness are correlated (0.67), so the individual coefficients should be interpreted with care. This describes expected miles of buyers; it is not a model for predicting who will buy.

### Fitted regression equation

$$\text{Miles}_{\text{predicted}} = -56.74 + 20.21 \times \text{Usage} + 27.21 \times \text{Fitness}$$

## Answers to the guiding questions

1. **What types of variables are in the data?** Six numeric (Age, Education, Usage, Fitness, Income, Miles; Usage and Fitness are ordinal scores) and three categorical (Product, Gender, MaritalStatus). There are no missing values.
2. **What do the distributions look like?** Age, Income and Miles are right-skewed. Most customers are in their 20s, plan to use the treadmill 3-4 times a week and rate their fitness as 3.
3. **Which product is more popular among males or females?** TM195 is the most purchased overall. Women and men buy TM195 and TM498 in near-equal numbers, but TM798 is bought mostly by men (33 of 40).
4. **Is purchase affected by marital status?** No. About 60% of buyers of every product are partnered (chi-square p = 0.96).
5. **Are there significant correlations?** Yes: Fitness-Miles (0.79), Usage-Miles (0.76), Usage-Fitness (0.67), Education-Income (0.63). Age is almost uncorrelated with the usage variables.
6. **What is the distribution of miles by product?** TM195 and TM498 are similar (median 85 miles per week each); TM798 is much higher (median 160), with the widest spread and the largest values (up to 360).

## Recommendations

1. **Position the TM798 as the premium, performance product** for higher-income (about $75k+), highly fit customers who plan to train around 5 times a week, rather than marketing it broadly.
2. **Investigate the gender gap on TM798.** Only 18% of its buyers are women, versus about half for the other models. The data cannot say why (marketing reach, features, or preference), so test it with targeted campaigns and customer surveys before drawing conclusions.
3. **Treat TM195 and TM498 as one casual segment.** Their buyers are very similar on every measured attribute, and statistically indistinguishable on income, usage, fitness and miles. Make the difference between the two clear to customers through features or price, or consider simplifying the line-up. (This dataset has no price or feature information, so this needs to be tested.)
4. **Do not segment by age or marital status.** Neither separates the products.
5. **Guide buyers at the point of sale** with two questions, planned weekly usage and fitness level; they are the strongest signals of which tier a customer ends up buying.
6. **Collect richer data next:** non-buyers, price paid, satisfaction and repeat purchases, which would allow modelling of *who buys*, not just describing who bought.

## Limitations

- **Small sample:** 180 customers from a three-month window at CardioGood Fitness retail stores. Some segments are tiny (for example 7 women bought a TM798).
- **Buyers only:** the profiles describe who bought each product, not the wider market, and cannot show what *drives* a purchase.
- **Self-reported and forward-looking:** Usage, Miles and Fitness are customers' own expectations and ratings, not measured behaviour.
- **Overlapping drivers:** Income, Education, Fitness and Usage are correlated, so product differences on these cannot be cleanly separated from each other.
- **Association, not causation**, throughout. Outliers in Miles (above 200, mostly TM798 buyers) were kept in the analysis.
