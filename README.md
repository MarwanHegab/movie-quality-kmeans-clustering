# Movie Dataset: A K-Means Clustering Analysis

We use K-means clustering
to partition movies from the [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)
(Kaggle) into interpretable groups based on six numeric features: budget,
revenue, runtime, popularity, average rating, and vote count.

**Authors:** Shreyes Balaji, Marwan Hegab, Mazin Hussein, Mikael Rotberg,
Roberto Rubio, Jordan Woda

## Summary

- Merged `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` on movie ID: 4,803 raw movies.
- Cleaning: recoded zero-valued budget/revenue/runtime as missing (TMDB's
  convention for "unknown") — 1,037 missing budgets, 1,427 missing revenues;
  dropped rows with any missing value (4,803 → 3,229 rows, no duplicate IDs);
  removed 597 outlier rows via Tukey's IQR rule — leaving 2,632 clean movies.
- 70/30 train/test split: 1,842 train / 790 test movies. Features standardized
  using training-set mean/SD only (no test-set leakage).
- K chosen via elbow, silhouette, and gap-statistic diagnostics, all agreeing on **K = 4**.
- Final model (`kmeans`, nstart = 25): cluster sizes 608 / 592 / 322 / 320,
  between/total SS ratio = 0.4953 on the training set.
- Test-set evaluation: new movies assigned to their nearest training centroid
  (via `pdist`); cluster proportions were nearly identical between train and
  test (e.g. Cluster 1: 33.0% train vs 35.8% test), and average silhouette
  width on the test set was 0.203 — evidence the four-cluster structure
  generalizes to unseen movies rather than overfitting.

| Cluster | Size | Budget | Revenue | Runtime | Popularity | Rating | Votes | Profile |
|---|---|---|---|---|---|---|---|---|
| 1 | 608 | $14.6M | $27.7M | 112.9 | 13.0 | 6.74 | 276 | Prestige mid-budget drama |
| 2 | 592 | $26.1M | $39.9M | 97.3 | 13.1 | 5.49 | 255 | Lower-rated mainstream releases |
| 3 | 322 | $26.0M | $98.4M | 108.9 | 41.5 | 6.83 | 1,319 | Fan-favorite hits |
| 4 | 320 | $67.7M | $171.7M | 113.7 | 30.6 | 6.14 | 890 | Big-budget blockbusters |

K-means is effective here because it finds structure without labels, but it's
sensitive to variable selection, scaling, and the pre-chosen K. See the essay
for the full diagnostics (diagonal plot, elbow/silhouette/gap plots, PCA
cluster visualization) and discussion of limitations.

## Files

- [`project_5_essay.pdf`](project_5_essay.pdf) — full write-up (introduction, data
  description, cleaning, analysis, model evaluation, conclusion, references).
- [`analysis.Rmd`](analysis.Rmd) — R Markdown source. Knit it to reproduce a full
  HTML report: data cleaning, diagnostics, cluster fitting/visualization, and
  test-set evaluation.

## Getting the data

This project uses the [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)
from Kaggle, which isn't bundled in this repo (Kaggle requires a login to
download, and its redistribution terms are less clear-cut than the UCI
datasets used in the other projects). To reproduce this analysis:

1. Download `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` from the Kaggle link above.
2. Place both files in a `data/` folder inside this project directory.

## Reproducing the analysis

```r
install.packages(c("dplyr", "tidyr", "ggplot2", "readr", "cluster",
                    "factoextra", "GGally", "knitr", "scales", "pdist", "rmarkdown"))
```

With the two CSVs in `data/`, open this folder as your working directory
(e.g. open `project_5/` in RStudio) and knit `analysis.Rmd` (Knit button, or
`rmarkdown::render("analysis.Rmd")`).

## Reference

1. Kaggle — TMDB 5000 Movie Dataset: https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata
