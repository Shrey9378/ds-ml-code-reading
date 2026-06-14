# Answers – tips_01_eda.py

## Q1 · business.objective-alignment

The regression slope is approximately 0.105, meaning that for every 1 unit increase in total bill, the tip increases by about 0.105 units. This implies that customers typically tip around 10–11% of the bill value on average.

The script engineers tip_pct because raw tip values are strongly dependent on bill size. Using tip alone mixes generosity with spending level. tip_pct normalizes the data and allows analysis of tipping behavior independent of bill amount, making comparisons across groups more meaningful.

## Q2 · business.decision-readiness

From the summary statistics, sex shows the least difference in median tip_pct:

Male: 15.35%
Female: 15.56%

The difference is very small compared to other variables.

A small median difference does not mean the predictor should be excluded. It only indicates weak univariate separation. The variable may still be important in interaction effects (e.g., sex × smoker) or in combination with other features in a multivariate model.

## Q3 · eda.plot-assumption

Strip plots are useful because they show individual data points, while box plots only show summary statistics (median, quartiles, and outliers). Strip plots reveal the actual distribution, clustering, and overlap of values within each category.

This is important because two groups can have similar medians but very different internal distributions, which box plots alone cannot show.

Yes, the visual layout could be improved by:

adding jitter to reduce overlap
reducing marker size and increasing transparency
or combining violin plots with strip overlays for better density understanding. 

## Q4 · eda.confounding

From the results:

Highest average total_bill: Sunday (19.63)
Next: Saturday (18.24)

Since higher bills often lead to higher absolute tips and may influence tip_pct behavior, the observed day differences in tipping could be partly explained by bill size differences across days.

Therefore, bill size acts as a potential confounding variable, meaning the “day effect” on tipping may not be purely due to day itself but due to differences in spending patterns on those days.

## Q5 · eda.confounding

If rows in the smoker × sex heatmap were not monotonic or showed inconsistent patterns across columns, it would indicate an interaction effect. This means the effect of smoking status on tip_pct depends on sex (or vice versa), rather than being independent.

In the actual heatmap:

Smokers: Male = 15.28, Female = 18.22
Non-smokers: Male = 16.07, Female = 15.69

This shows a mild interaction effect, as the difference between sexes changes depending on smoker status.

## Q6 · eda.confounding

The results show that the sex difference in tip_pct changes across party sizes, but there is no consistent or strong gap within each size category.

This means that the apparent sex difference in tipping is largely reduced or disappears once party size is controlled for. Therefore, the observed overall difference is likely due to confounding by party size, rather than a direct effect of sex on tipping behavior.