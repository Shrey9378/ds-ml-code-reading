# Answers – tips_01_eda.py

## Q1 · business.objective-alignment

**Ans** The regression slope is approximately 0.105, meaning that for every 1 unit increase in total bill, the tip increases by about 0.105 units. This implies that customers typically tip around 10–11% of the bill value on average.

The script engineers tip_pct because raw tip values are strongly dependent on bill size. Using tip alone mixes generosity with spending level. tip_pct normalizes the data and allows analysis of tipping behavior independent of bill amount, making comparisons across groups more meaningful.

## Q2 · business.decision-readiness

**Ans** From the summary statistics, sex shows the least difference in median tip_pct:

Male: 15.35%
Female: 15.56%

The difference is very small compared to other variables.

A small median difference does not mean the predictor should be excluded. It only indicates weak univariate separation. The variable may still be important in interaction effects (e.g., sex × smoker) or in combination with other features in a multivariate model.

## Q3 · eda.plot-assumption

**Ans** Strip plots are useful because they show individual data points, while box plots only show summary statistics (median, quartiles, and outliers). Strip plots reveal the actual distribution, clustering, and overlap of values within each category.

This is important because two groups can have similar medians but very different internal distributions, which box plots alone cannot show.

Yes, the visual layout could be improved by:

adding jitter to reduce overlap
reducing marker size and increasing transparency
or combining violin plots with strip overlays for better density understanding. 

## Q4 · eda.confounding

**Ans** From the results:

Highest average total_bill: Sunday (19.63)
Next: Saturday (18.24)

Since higher bills often lead to higher absolute tips and may influence tip_pct behavior, the observed day differences in tipping could be partly explained by bill size differences across days.

Therefore, bill size acts as a potential confounding variable, meaning the “day effect” on tipping may not be purely due to day itself but due to differences in spending patterns on those days.

## Q5 · eda.confounding

**Ans** If rows in the smoker × sex heatmap were not monotonic or showed inconsistent patterns across columns, it would indicate an interaction effect. This means the effect of smoking status on tip_pct depends on sex (or vice versa), rather than being independent.

In the actual heatmap:

Smokers: Male = 15.28, Female = 18.22
Non-smokers: Male = 16.07, Female = 15.69

This shows a mild interaction effect, as the difference between sexes changes depending on smoker status.

## Q6 · eda.confounding

**Ans** The results show that the sex difference in tip_pct changes across party sizes, but there is no consistent or strong gap within each size category.
The scatter plot and size-specific box plots suggest that the difference in tip percentage between males and females becomes much smaller when party size is held constant.

Within individual party-size groups, male and female customers tend to have similar tip percentages. Therefore, the apparent sex effect largely disappears after controlling for party size.

This indicates that party size acts as a confounding variable and explains much of the observed difference between sexes.


## Answers - tips_02_eda.py

## Q1 · pipeline.data-leakage

Q: Cell [4] computes mu and sigma from X_train only. What would happen if you used the full X instead? Would the output numbers change noticeably? Would the test score be trustworthy?


**Ans** Using the full dataset X means the test data would influence the standardization parameters (mu and sigma).
This is called data leakage because information from the test set leaks into the training pipeline.
The values of mu and sigma would be slightly different because they would include both train and test samples.
The model coefficients and test metrics might change slightly, especially on small datasets.
The test score would no longer be fully trustworthy because the test set has indirectly influenced model training.

## Q2 · transform.trace-pred

Q: Cell [4]: X_train_b = add_bias(X_train_s). What is the shape of X_train_b? Why 2 columns when total_bill is a single number?


**Ans** 
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

This means that the apparent sex difference in tipping is largely reduced or disappears once party size is controlled for. Therefore, the observed overall difference is likely due to confounding by party size, rather than a direct effect of sex on tipping behavior.



## Q3 · OLS vs Gradient Descent

**Ans** Same results in linear regression:
  OLS = GD

Example:
intercept=3.0878, slope=0.9357

Reason:
- Convex loss → single global minimum

Difference only if:
- high LR
- few iterations
- poor scaling
- numerical issues



## Q4 · Algorithm

**Ans** Optimizer: Batch Gradient Descent

Gradient:
grad = ∂J(θ)/∂θ

Computed as:
grad = X.T @ (pred - y) / n



## Q5 · Learning Rate

- α = 0.01 → slow, stable
- α = 0.99 → unstable / divergence

Detection:
- oscillation → LR too high
- flat slow curve → not converging



## Q6 · Convergence Plot

- Red dashed line = final minimum MSE

Missing:
- validation set
- test loss tracking



## Q7 · Multi-feature extension

- Input: X = [total_bill, size]
- Shape: (195,2) → bias → (195,3)

Gradient descent:
- same formula
- more parameters only

OLS:
θ = (XᵀX)^(-1) Xᵀy


##  KEY IDEA

- OLS = exact solution
- GD = iterative solution
- Both converge if properly tuned

## Answers - tips_03_all_features.py

## Q1 · deployment.input-assump

**Ans** pd.get_dummies() assumes that all possible categories are already present in the training data. It creates dummy variables only for the categories it sees during training.

For example:

sex → Male, Female
day → Thur, Fri, Sat, Sun

The model assumes future data will contain the same categories.

If a new category appears during prediction (for example day = "Holiday"), the model has no corresponding dummy column. This can lead to a feature mismatch between training and prediction data and may cause prediction errors. This is known as the unseen category problem.


## Q2 · transform.feature-engineering

**Ans** drop_first=True removes one dummy variable from each categorical feature and uses it as the reference category.

Example:

Without drop_first:

day_Thur
day_Fri
day_Sat
day_Sun

With drop_first=True:

day_Fri
day_Sat
day_Sun

Here, Thur becomes the reference category.

If drop_first is removed, all dummy variables are kept. This creates perfect multicollinearity because one dummy column can always be determined from the others. OLS coefficients become non-unique and harder to interpret, although predictions usually remain similar.


## Q3 · transform.trace-pred

**Ans** X.values converts the pandas DataFrame into a NumPy array.

The following information is lost:

- Column names
- Feature labels
- Index labels
- DataFrame metadata

Only the numerical values remain. This makes later interpretation more difficult because features must be tracked by their column positions.



## Q4 · process.reproducibility

**Ans** Without random_state=42, a different train/test split would be generated every time the script runs.

The following outputs would change:

- Model coefficients
- Train R²
- Test R²
- Permutation importance values
- Residual analysis results

The dataset itself and preprocessing steps would remain unchanged.

Model evaluation should be deterministic because reproducible results are necessary for reliable comparisons and scientific validity.

A better approach is to define:

RANDOM_STATE = 42

at the top of the script and use it everywhere randomness is required.



## Q5 · business.objective-alignment

**Ans** MSE is an appropriate loss function for predicting tip amounts because the target variable is continuous.

MSE penalizes large prediction errors more heavily and is commonly used in regression models.

If the goal were to identify unusual or outlier tippers, prediction accuracy would no longer be the primary objective. In that case anomaly detection methods could be used and evaluation metrics such as Precision, Recall, or F1-score would be more appropriate.


## Q6 · evaluation.overfit

**Ans** A small difference between train R² and test R² suggests that overfitting is limited, but it does not prove that the model is not overfitting.

The model may still be underfitting if it performs similarly poorly on both datasets.

Another reason for the small train/test gap is that the script uses a simple LinearRegression model with only a few predictors. Simple models generally have a lower tendency to overfit compared with more complex models.


## Q7 · deployment.dist-shift

**Ans** If the model is deployed in a different restaurant, several features may behave differently:

- total_bill
- tip
- size
- day
- time
- smoker status

Different restaurants may have different pricing structures, customer demographics, and tipping cultures.

To detect performance degradation after deployment, I would:

- Monitor RMSE and R² over time.
- Compare predicted tips with actual tips.
- Track residual distributions.
- Compare production feature distributions with training distributions.

Significant changes would indicate distribution shift and may require model retraining.


## Q8 · business.objective-alignment

**Ans** Predicting raw tip amount may not be the most interesting business question because tip amount is strongly influenced by total_bill.

The script contains a commented feature-engineering line:

y = df["tip"].values / X["total_bill"].values * 100

This creates tip_pct (tip percentage).

Tip percentage is often a better target because it measures customer generosity independently of bill size.

If tip_pct is used instead of tip:

- R² would likely decrease.
- total_bill would become less dominant.
- Variables such as sex, smoker status, day, time, and size may become relatively more important.
- The model would focus on explaining tipping behaviour rather than spending behaviour.

Therefore, predicting tip percentage may provide more meaningful behavioural insights than predicting raw tip amount.