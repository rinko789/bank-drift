# Does a bank marketing model survive the financial crisis?

Semester project, Advanced Data Analytics, HEC Lausanne, autumn 2026.
Author: Omar Jebril Oueslati.

## Research question

Does a model trained on the 2008 telemarketing calls of a Portuguese bank still predict
term-deposit subscriptions on the 2009–2010 calls, after the Euribor rate collapsed?
And does a flexible model (random forest) lose more performance than a simple one
(logistic regression)?

**Design.** The calls are cut into four time blocks (2008, Mar–May 2009, Jun–Dec 2009, Mar–Nov 2010).
Models are trained at three dates (end of 2008, end of May 2009, end of 2009), each time on every call
known at that date, and tested on every later block. This shows how long a model trained before the
crisis stays useful, and how much updating it helps.

## Data

`data/raw/bank-additional-full.csv`: Bank Marketing data set (41,188 calls, May 2008 to
November 2010), UCI Machine Learning Repository, https://doi.org/10.24432/C5K306.
Licence: Creative Commons Attribution 4.0 (CC BY 4.0).

Moro, S., Cortez, P. and Rita, P. (2014). A Data-Driven Approach to Predict the Success of
Bank Telemarketing. *Decision Support Systems*, 62, 22–31.

## Repository layout

```
data/raw/          original file, unchanged
data/processed/    data with reconstructed dates and periods (notebook 01), model predictions (notebook 03)
notebooks/         one notebook per stage, run in numeric order
src/data_utils.py  shared code: loading, date reconstruction, period boundaries
src/modelling.py   shared code: time blocks, training windows, features, model definitions
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
| `03_models.ipynb` | fit and tune logistic regression and random forest at three dates | Table 3, `data/processed/predictions.csv` |

Running time: about three minutes in total, almost all of it in notebook 03 (measured on a 4-core machine). No other resource is needed.

## Declaration of tools

This section is also reproduced in the appendix of the paper (Requirements 4.5(9), 5.2 and 6.6).

**Claude (Anthropic)** — generative AI assistant, used through claude.ai and Claude Code.

1. **Initial version of the repository.** Claude generated the first version of all six project files at the
   student's request: `README.md`, `requirements.txt`, `.gitignore`, `src/data_utils.py`,
   `notebooks/01_prepare_data.ipynb` and `notebooks/02_explore_time.ipynb`. This includes the data loading,
   the reconstruction of the year of each call, the definition of the four time periods, the removal of
   `duration`, Figure 1 and Tables 1–2.
2. **Review and corrections (commits authored by "Claude" in the history).** Claude re-ran both notebooks from
   a fresh clone, corrected inaccurate statements in their text (months with no calls, the description of
   the validation period, the date the Euribor fell below 1%), changed Figure 1 so that its lines break at
   months with no calls, added a check for duplicate rows and a table of the macro-economic variables in
   the training period, and saved the notebooks with their outputs.
3. **Modelling (notebook 03 and `src/modelling.py`).** After the first version of notebook 03 showed that
   a model trained on 2008 performs at chance level even before the crisis, Claude proposed the
   expanding-window design (training at three dates), which the student chose. Claude wrote
   `src/modelling.py` and `notebooks/03_models.ipynb`; the choice of columns (leaving out the earlier-campaign
   columns at first, then `pdays` and `month`; keeping the economic columns with a version without them)
   was discussed with and decided by the student.
4. **Interpretation text.** Claude drafted the interpretation of the client-mix table (last cell of
   `notebooks/02_explore_time.ipynb`) and the commentary on Figure 1.

The data set is not AI-generated: it is the original UCI file cited above. No code was taken from other
repositories, forums or students. The student remains responsible for all submitted work (Requirement 5.4).
