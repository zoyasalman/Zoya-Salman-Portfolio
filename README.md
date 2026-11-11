# U.S. Obesity and Food Environment Analysis

**Where should a public-health department place food access programs?** A state-level analysis combining 14 years of CDC survey data with USDA food environment indicators to show where adult obesity is concentrated, who it affects, and which neighborhood conditions travel with it.

**Team B09:** Bhavya Kandhari · Zoya Salman · Danyang Qin (Lucy) · Sanjay Kumar · Dylan Rubenstein
**Course project:** MSBA data analytics (Python, pandas, scikit-learn, seaborn)

![Adult obesity trend, 2011-2024](images/obesity_trend_2011_2024.png)

---

## The Problem

Adult obesity is linked to type 2 diabetes, heart disease and certain cancers, and rates vary widely by state, income, education, age and race. Our intended client is a **state or regional public-health department** with a limited budget for programs such as SNAP incentives, healthy-store initiatives and community recreation. Its decision is which states (and then which counties) to prioritize, and what type of program fits each place.

> **Research question:** How are food environment, socioeconomic status, demographic characteristics and geographic location associated with adult obesity rates across the U.S., and where should a health department place food access programs?

## Key Findings

| # | Finding |
|---|---------|
| 1 | Adult obesity rose from **27.4% (2011) to 33.1% (2024)**. In 2022, state rates ranged from **24.3% (DC) to 41.0% (West Virginia)**, and 22 states were at 35% or higher, mostly in the South and Appalachia. |
| 2 | The geographic gap is **not just an older-adult story**. High-obesity states are about 9 points higher among 18-24 year-olds, widening to nearly 14 points by middle age. |
| 3 | Wealthier states have lower obesity (**r = -0.71**), yet adults in the *same income bracket* differ by **17.4 points** depending on their state. Even higher earners and college graduates in high-obesity states stay above the national average. |
| 4 | More **dollar stores** per capita go with higher obesity (**r = +0.62**). More **full-service restaurants** (**r = -0.69**) and **recreation facilities** (**r = -0.49**) go with lower obesity. |
| 5 | Nine food-environment factors alone explain about **69% of the variation** in state obesity (R² = 0.69), but the model misses some states, which points to other drivers. |

![State obesity ranking, 2022](images/state_obesity_ranking_2022.png)

![Food environment correlations with obesity](images/food_environment_correlations.png)

## Recommendations

1. **Prioritize high-obesity states first.** Start with West Virginia, Louisiana and Oklahoma, then expand to other states at 35% or higher. High dollar-store density and few full-service restaurants can flag states that need closer attention.
2. **Reach whole communities, not just low-income residents.** Higher earners in high-obesity states are still above the national average.
3. **Improve existing stores and activity options** (SNAP incentives, healthy-store programs, affordable fitness, parks and safe walking spaces) rather than relying on new supermarkets or awareness campaigns alone.
4. **Target the groups with the greatest need:** Black adults, American Indian/Alaska Native adults, adults 45-54, and adults without a high school diploma. Use mobile markets, transportation support and school meals for groups with low food access.
5. **Start early and pilot in partnership.** The gap already appears at ages 18-24, so involve schools, universities and workplaces, and work with CDC, USDA, local governments, retailers and community organizations.

## Data

| Dataset | Source | Size | Used for |
|---------|--------|------|----------|
| **CDC BRFSS Nutrition, Physical Activity & Obesity** (2011-2024) | [data.gov](https://catalog.data.gov/dataset/nutrition-physical-activity-and-obesity-behavioral-risk-factor-surveillance-system?from_hint=eyJzb3J0IjoicG9wdWxhcml0eSJ9) | 110,880 rows x 33 columns, 55 locations | Obesity, overweight, inactivity and diet indicators by state, year, income, education, age, sex and race |
| **USDA Food Environment Atlas** (2025) | [USDA ERS](https://ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads) (current version section) | 957,753 rows, 304 variables, 3,156 counties | Store, restaurant, food access, health and socioeconomic indicators |

The data files are not stored in this repository. Download them from the links above (see [How to Run](#how-to-run)).

## Methodology

1. **Clean BRFSS** (110,880 to 97,666 rows): dropped 13,214 rows with no estimate (small samples), removed unused columns, converted sample size to numeric, renamed columns and mapped question IDs to short indicator labels (obesity, overweight, no activity, fruit and vegetable intake). No duplicates found.
2. **Clean Food Environment Atlas:** converted placeholder codes (`-9999`, `-8888`) to missing values, removed rows with no FIPS code, kept five relevant categories (access, stores, restaurants, health, socioeconomic), and corrected variables mislabeled as "Count" that are really per-1,000-population rates.
3. **Roll counties up to states:** summed count variables and averaged percent, per-1,000 and dollar variables, then pivoted from long to wide (51 rows x 111 columns).
4. **Merge** BRFSS and Atlas on state (`many_to_one` validation) for all 50 states and DC.
5. **Explore:** 15 charts covering trends, state rankings, demographic breakdowns, same-income comparisons, correlation analysis and state profiles (West Virginia, Vermont, Colorado).
6. **Model:** multiple linear regression of 2022 state obesity on nine food-environment factors, evaluated by R² and mean absolute error, with a look at which states the model over- and under-predicts.

![Predicted vs. actual obesity](images/model_predicted_vs_actual.png)

## Limitations

- **Correlation, not causation.** Income, urbanization and age are tangled together at the state level.
- **Unweighted county averages** and high missingness for some variables (especially Alaska and parts of Connecticut).
- **Mismatched years** between BRFSS and Atlas variables (charts use 2021, 2022 and 2024 depending on the question).
- **Self-reported height and weight** in BRFSS tends to understate obesity.
- **Small groups** have wide confidence intervals.

## Future Work

- Add county-level obesity data (e.g., CDC PLACES) to match the Atlas at county level.
- Test whether healthy-store or recreation programs actually change obesity over time.
- Add variables such as transit access, walkability, healthcare availability and food prices.
- Weight county-to-state aggregation by population.

## Repository Structure

```
.
├── README.md
├── images/                                      # charts used in this README
├── B09_Obesity_and_Food_Environment_Analysis.ipynb   # full analysis notebook
└── data/                                        # not included; see "How to Run"
    ├── BRFSS_Obesity.csv
    ├── StateAndCountyData.csv
    └── VariableList.csv
```

## How to Run

1. Clone the repo and install dependencies:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn jupyter
   ```
2. Download the datasets from the links in the [Data](#data) section. Save the BRFSS file as `data/BRFSS_Obesity.csv`, and `StateAndCountyData.csv` and `VariableList.csv` from the USDA download into `data/`.
3. Open the notebook and run all cells from top to bottom. The setup cell uses `./data/` locally, or a Google Drive path when run in Colab.

## Tools

Python · pandas · NumPy · matplotlib · seaborn · scikit-learn (LinearRegression)

## Generative AI Disclosure

We used Claude for grammar review, debugging, adapting chart templates, choosing a consistent palette, readable variable labels, and revising the wording of the narrative and recommendations. The chart ideas, analysis, interpretations and recommendations are our own, and we verified all code outputs ourselves.
