# best-fried-rice-data-project
Statistical analysis, Bayesian weighted scoring, to determine the best best rice recipe. Interactive Power BI dashboard.
---

## Summary
I made this project to determine the statistically best fried rice recipe. I started with a dataset from [food.com]([url](https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews?resource=download)) of recipes and reviews. I then analzyed it in Jupyter notebook trying to filter to only the fried rice recipes, common ingredients, determined highest lift (how ingredients effect recipe) and handling small-sampled skewed recipes. 

I ran into a problem of ingredient lists in the raw food.com dataset are stored as serialized R-style string vectors. Free-text ingredients use varied synonyms such as "scallion", "green onion", "spring onion", or compound terms. I used engineered a dictionary of regex tokens mapping variations to standardized binary indicator flags (1/0) across major categories.

Also, seeing an ingredient is common doesn't prove it improves the dish so I used the lift = Xw/ - Xw/o formula. Because group sample sizes and variances range, I applied Welch’s Two-Sample t-Test to calculate p-values.

Purpose of OLS multivariable regression: In a real recipe, ingredients do not exist in isolation. If a cook uses oyster sauce, they almost always use soy sauce and egg as well. A bivariate lift could suffer from omitted variable bias or confounding, so I estimated an OLS regression using statsmodels to observe the marginal effect of each ingredient holding all other variables constant.

Finally, I exported important files and put it in Power BI, unfortunately I dont have premium so I cant share the interactive dashboard.


### Key Insights:
- **Leading flavour driver:** Oyster sauce yields the highest positive rating lift (+0.11, p < 0.01), followed closely by soy sauce and eggs.
- **The core seasoning ratio:** Top-tier recipes consistently converge toward a **0.9:1 Soy Sauce to Oyster Sauce** volumetric ratio.
- **Mitigating sample bias:** Unweighted ratings skewed toward niche outliers; Bayesian shrinkage successfully regularized ranking stability without penalizing high-volume community favorites.

---

## Tech stack and methodology

| Layer | Tools | Focus Areas |
| :--- | :--- | :--- |
| **Data Engineering** | Python (`pandas`, `re`) | Text parsing, tokenization, binary feature engineering |
| **Statistical Analysis** | Python (`scipy.stats`, `numpy`) | Bayesian weighted scoring, two-sample t-tests, effect size |
| **Business Intelligence** | Microsoft Power BI | DAX aggregations, interactive tile slicers, executive reporting |

---

## Mathematical approach

### 1. Bayesian weighted scoring
To balance high ratings against low review counts, recipes were regularized using an IMDb-style Bayesian mean:
WR = (v / v+m)*R + (m / v+m)*C

- W = weighted recipe rating
- R = observed recipe average rating
- v = # of reviews for the recipe
- m = min review threshold parameter (set to 75th percentile of distribution)
- C = mean rating across the entire dataset ($\approx 4.66$)

## Final Ingredients:
- 3.5 cups of rice
- 2.0 tnsp of soy sauce
- 2.25 tbsp of oyster sauce
- 2-3 eggs
- 3 scallions



```text
├── best-fried-rice.ipynb         # Full Python pipeline (ETL, scoring, hypothesis testing)
├── fried_rice_processed.csv      # Processed dataset with binary ingredient flags & scores
├── ingredient_lift.csv           # Statistical lift, sample frequencies, and significance flags
├── Screenshot 2026-10-14...png   # High-resolution dashboard visual asset
├── rough notes.pdf               # Initial mathematical formulations and exploratory notes
└── README.md
