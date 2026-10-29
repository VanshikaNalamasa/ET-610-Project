# Predicting Moment of Learning in ASSISTments

**ET-610 Project | Learning Analytics and Educational Data Mining**

A study of how accurately the probability of a *moment of learning* can be predicted using a reduced feature set, and how prediction performance relates to student behavioral profiles.

## Overview

The moment-of-learning probability, \(P(J)\), estimates the likelihood that a student has just learned a skill during a practice opportunity.

This project investigates three questions:

- **Model simplicity vs. accuracy:** How much predictive performance is lost when using only correctness and hint usage instead of a richer set of features?
- **Student behavioral profiles:** How do patterns of help-seeking, correctness, and response time relate to \(P(J)\)?
- **Performance across profiles:** Does the accuracy gap between reduced and full models differ between behavioral groups?

The analysis uses the ASSISTments \(P(J)\) dataset and combines exploratory data analysis, temporal analysis, student clustering, and supervised regression.

## Key Findings

- The reduced model, using only correctness and hint-related features, achieved an \(R^2\) of approximately **0.09**.
- The full-feature model achieved an \(R^2\) of **0.21 with Ridge Regression** and **0.35 with Histogram-Based Gradient Boosting**.
- K-means clustering identified two exploratory behavioral profiles: a high-help-seeking group (approximately 15% of clustered students) and a low-help-seeking group (approximately 85%).
- The high-help-seeking group had a lower mean \(P(J)\), but no statistically significant difference in spikiness was observed.
- The full model outperformed the reduced model in both profiles. The analysis found no statistically distinguishable difference in the accuracy gains between the two groups, although the smaller group limits statistical power.

The findings also highlight a potential limitation: some history-based features may overlap with information used in constructing the \(P(J)\) target.

## Repository Structure

```text
ET-610-Project-/
├── ET610_Project.ipynb
├── outputs/
│   ├── figures/
│   └── tables/
└── ET610_Project_Report.pdf
```

- **`ET610_Project.ipynb`** — Jupyter notebook containing the data analysis, feature preparation, modeling, clustering, and evaluation workflow.
- **`outputs/figures/`** — Figures generated for exploratory analysis, behavioral profiles, and model evaluation.
- **`outputs/tables/`** — Tables containing descriptive statistics, model metrics, and statistical comparisons.
- **`ET610_Project_Report.pdf`** — Final project report describing the methodology, results, interpretation, and limitations.


## Methodology

### 1. Data Preparation and Exploration
- Examined attempt-level student interaction data.
- Inspected missing target labels, response times, correctness, hint usage, and practice-opportunity counts.
- Analyzed the distribution of \(P(J)\) and its relationship with correctness and hint use.

### 2. Temporal Analysis
- Examined hint rate, error rate, and median response time across successive practice opportunities.
- Compared temporal patterns across the identified behavioral profiles.

### 3. Behavioral Profiling
- Aggregated interaction data at the student level.
- Applied feature standardization and K-means clustering.
- Selected the number of clusters using silhouette analysis.
- Compared profile-level \(P(J)\) and spikiness using statistical tests.

### 4. Predictive Modeling
Compared three feature sets:

| Feature set | Description |
|---|---|
| Reduced | Correctness and hint-related features only |
| Full | All 54 allowed predictive features |
| Full minus high-risk | Full feature set excluding 22 potentially high-risk features |

Two regression approaches were evaluated:

- Ridge Regression
- Histogram-Based Gradient Boosting

Evaluation used a student-level train/test split to keep students separate between training and testing, along with student-grouped cross-validation.

### 5. Evaluation and Robustness Checks
- Compared \(R^2\), Mean Absolute Error (MAE), and Root Mean Squared Error (RMSE).
- Used student-level bootstrap confidence intervals to assess model performance differences.
- Examined permutation feature importance and potential target leakage.
- Evaluated held-out predictions separately across behavioral profiles.

## Results Summary

| Model | Feature set | Test \(R^2\) |
|---|---|---:|
| Ridge Regression | Reduced | 0.0876 |
| Histogram-Based Gradient Boosting | Reduced | 0.0880 |
| Ridge Regression | Full | 0.2078 |
| Histogram-Based Gradient Boosting | Full | 0.3539 |
| Ridge Regression | Full minus high-risk | 0.1854 |
| Histogram-Based Gradient Boosting | Full minus high-risk | 0.3481 |

The full-feature models substantially outperformed the reduced models. Removing the potentially high-risk features caused only a modest decline in performance, particularly for gradient boosting. However, the remaining history features may still contain information related to the construction of the target.

## Tools and Technologies

- Python
- Jupyter Notebook
- pandas and NumPy
- SciPy
- scikit-learn
- Matplotlib

## Reproducibility

1. Clone or download this repository.
2. Open `ET610_Project.ipynb` in Jupyter Notebook or Google Colab.
3. Make the ASSISTments dataset available at the location expected by the notebook.
4. Run the notebook cells in sequence to reproduce the analysis and model evaluation.

The original dataset is not assumed to be included in this repository. Dataset access and file paths may need to be configured before running the notebook.

## Limitations

- A substantial proportion of attempts have missing \(P(J)\) labels, and labeled and unlabeled rows differ systematically.
- The target distribution is highly right-skewed.
- Behavioral profiles are exploratory and unbalanced.
- Some predictive features may overlap with information used to construct \(P(J)\).
- The smaller profile has limited test-set representation, reducing the power of comparisons between profiles.

## Authors

- **Vanshika Nalamasa** — IIT Bombay
- **Jasmitha Rongali** — IIT Bombay

## Acknowledgements

This project was completed as part of **ET-610**. We acknowledge the ASSISTments platform and the researchers whose work on Bayesian Knowledge Tracing and moment-by-moment learning motivated this analysis.

## References

Key references are documented in the final project report, including work on Knowledge Tracing, moment-of-learning prediction, student help-seeking, and the Python scientific computing ecosystem.
