# Answers – tips_01_eda.py

## Q1 · business.objective-alignment

The OLS slope is approximately 0.105, which means that for every $1 increase in the total bill, the expected tip increases by about $0.105. This corresponds to a typical tip rate of roughly 10.5%.

The script then creates `tip_pct = tip / total_bill × 100` because raw tip amounts are strongly influenced by bill size. A customer who spends more will usually leave a larger tip even if they tip at the same percentage rate. Using `tip_pct` normalizes for bill size and allows fair comparison of tipping behavior across customers.


## Q2 · business.decision-readiness

From the box plots, the predictor with the smallest difference in median tip percentage appears to be sex (male vs female). The medians are very similar and the distributions overlap heavily.

A small median difference does not automatically mean that the variable should be excluded from a model. A predictor may still contribute useful information through interactions with other variables, nonlinear effects, or by improving overall predictive accuracy when combined with other features.



## Q3 · eda.plot-assumption

The strip plots show the actual observations, while the box plots only summarize the distribution using quartiles, median, and outliers.

The strip plots reveal:

* The number of observations in each group.
* Clusters and gaps in the data.
* The spread of individual points.
* Whether apparent differences are driven by only a few observations.

Without the strip plots, two groups could have similar box plots while having very different underlying point distributions.

For visual improvement, I would slightly reduce point opacity, increase spacing between subplots, and possibly rotate category labels if needed. This would improve readability without changing the information shown.



## Q4 · eda.confounding

From the Q2 box plots, some days appear to have slightly higher tip percentages than others, particularly weekend days.

Cell [5] shows that total bill size also varies across days. Therefore, the observed day effect could be partly explained by differences in spending behavior. If customers spend more on certain days, the apparent day effect may actually be driven by bill size rather than the day itself. This is an example of potential confounding.


## Q5 · eda.confounding

If the rows in the smoker × sex heatmap were not monotone across columns, it would indicate an interaction effect. In other words, the effect of smoking status would depend on sex, or the effect of sex would depend on smoking status.

For example, if smoking increased tip percentage for males but decreased it for females, the trends would not be parallel and an interaction would be present.

The actual heatmap shows only small differences between groups, so any interaction appears weak rather than strong.



## Q6 · eda.confounding

The scatter plot and size-specific box plots suggest that the difference in tip percentage between males and females becomes much smaller when party size is held constant.

Within individual party-size groups, male and female customers tend to have similar tip percentages. Therefore, the apparent sex effect largely disappears after controlling for party size.

This indicates that party size acts as a confounding variable and explains much of the observed difference between sexes.


Q1 · pipeline.data-leakage

Q: Cell [4] computes mu and sigma from X_train only. What would happen if you used the full X instead? Would the output numbers change noticeably? Would the test score be trustworthy?


Using the full dataset X means the test data would influence the standardization parameters (mu and sigma).
This is called data leakage because information from the test set leaks into the training pipeline.
The values of mu and sigma would be slightly different because they would include both train and test samples.
The model coefficients and test metrics might change slightly, especially on small datasets.
The test score would no longer be fully trustworthy because the test set has indirectly influenced model training.

Q2 · transform.trace-pred

Q: Cell [4]: X_train_b = add_bias(X_train_s). What is the shape of X_train_b? Why 2 columns when total_bill is a single number?


X_train has shape (195,1) because there are 195 training samples and one feature (total_bill).
Standardization does not change the shape, so X_train_s is also (195,1).
add_bias() adds a column of ones for the intercept term.
Therefore:
X_train_b.shape = (195, 2)

The two columns are:

[1, standardized_total_bill]

Column 1 = bias/intercept term

Column 2 = feature value

Without the bias column, the model could only fit lines passing through the origin.





