# HDI Group Classification

Predicts which UNDP human development group a country belongs to (Low, Medium, High or Very High) from four indicators.

## Dataset
UNDP "Human Development Index - Full" file (Human Development Reports, hdr.undp.org). I used the 2021 values of four indicators for 159 countries:

- Life expectancy at birth
- Expected years of schooling
- GNI per capita
- Income inequality

The HDI score and mean years of schooling are **not** used as features. The group is defined from the HDI score, so using it would leak the answer.

## Method
1. Selected the 2021 columns from the 195 x 880 raw file.
2. Removed 36 countries with missing values (mostly missing income inequality), leaving 159.
3. Explored the data: histograms, box and violin plots, scatter plot, pairplot, correlation matrix, outliers (IQR rule) and VIF.
4. Stratified 80/20 split (127 train, 32 test) with scaling done inside a pipeline.
5. Compared six classifiers: Logistic Regression, KNN, Naive Bayes, Decision Tree, Random Forest and SVM, against a majority-class baseline of 34%.
6. Tuned with GridSearchCV and checked fairness with nested cross-validation.
7. Evaluated the final Random Forest on the test set and interpreted it with feature importance and an ablation test.

## Results
- Cross-validated accuracy: about 87-88% (nested cross-validation: 87.6% for Random Forest).
- Test accuracy: 93.75% (30 of 32 countries), 95% confidence interval 79.9% to 98.3%.
- Both mistakes were between neighbouring groups, for countries within 0.015 of an HDI cut-off.
- GNI per capita is the most important feature. Income inequality adds a small amount of information.

## Limitations
- Only 159 countries, so the test set is small and differences between top models are within noise.
- Life expectancy, schooling and GNI are inputs of the HDI formula, so high accuracy is partly expected.
- Countries with missing data were dropped, mostly rich or small states.
- One year only (2021).

## Built with
Python, pandas, NumPy, scikit-learn, matplotlib, seaborn.

## Files
- `hdi_classification.ipynb`: full notebook
- `Human_Development_Index_-_Full.csv`: dataset (optional, if you add it)

## Author
Keerthana J
