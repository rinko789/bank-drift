# Does a bank marketing model survive the financial crisis?

Semester project, Advanced Data Analytics, HEC Lausanne, autumn 2026.
Author: Omar Jebril Oueslati.

## Research question

Does a model trained on the 2008 telemarketing calls of a Portuguese bank still predict
term-deposit subscriptions on the 2009–2010 calls, after the Euribor rate collapsed?
And does a flexible model (random forest) lose more performance than a simple one
(logistic regression)?

## Data

`data/raw/bank-additional-full.csv`: Bank Marketing data set (41,188 calls, May 2008 to
November 2010), UCI Machine Learning Repository, https://doi.org/10.24432/C5K306.
Licence: Creative Commons Attribution 4.0 (CC BY 4.0).

Moro, S., Cortez, P. and Rita, P. (2014). A Data-Driven Approach to Predict the Success of
Bank Telemarketing. *Decision Support Systems*, 62, 22–31.

## Repository layout

```
data/raw/          original file, unchanged
data/processed/    data with reconstructed dates and periods (made by notebook 01)
notebooks/         one notebook per stage, run in numeric order
src/data_utils.py  shared code: loading, date reconstruction, period boundaries
figures/           figures used in the paper (made by the notebooks)
results/           tables used in the paper (made by the notebooks)
```

## How to run

```
pip install -r requirements.txt
```

Then run the notebooks in order, each from top to bottom in a fresh kernel:

| Notebook | Stage | Produces |
|---|---|---|
| `01_prepare_data.ipynb` | load, check, rebuild dates, assign periods, drop `duration` | `data/processed/bank_with_dates.csv` |
| `02_explore_time.ipynb` | exploration of the change over time | Figure 1, Tables 1–2 |

Running time: under one minute on the course workspace. No other resource is needed.
