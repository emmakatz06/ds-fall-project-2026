# Mental Health in Tech: Who Seeks Treatment?

A CUNY Tech Prep team project exploring the 2014 OSMI Mental Health in Tech Survey. We look at which personal and workplace factors are associated with tech workers reporting that they sought treatment for a mental health condition, and we present the results in an interactive Streamlit dashboard.

## Team

| Stage | Owner | Deliverables |
|---|---|---|
| 1. Data acquisition, cleaning & quality | Tony | Clean CSV, data dictionary, data quality report, preprocessing notebook |
| 2. Exploratory analysis & statistics | Emma | EDA notebook, statistical results, charts, findings summary |
| 3. Feature engineering, modeling & evaluation | Mohammad | Modeling notebook, evaluation metrics, model interpretation |
| 4. Visualization, app & integration | Malaak | Streamlit app, presentation, final documentation |

Each stage builds on the one before it. Everyone downstream of stage 1 should load `data/processed/survey_clean.csv`, not the raw file.

## Repository structure

```
data/
  raw/survey.csv               original OSMI download (never edited)
  processed/survey_clean.csv   cleaned dataset (output of stage 1)
notebooks/
  01_data_cleaning.ipynb       stage 1: cleaning walkthrough
  02_eda.ipynb                 stage 2 (to be added)
  03_modeling.ipynb            stage 3 (to be added)
app/                           stage 4: Streamlit dashboard (to be added)
docs/                          data dictionary, quality report, other docs
```

## The data

**Source:** [OSMI Mental Health in Tech Survey (2014)](https://www.kaggle.com/datasets/osmi/mental-health-in-tech-survey), from Open Sourcing Mental Illness (OSMI), a nonprofit focused on mental health in the tech industry. Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), so any work we share that uses this data must credit OSMI and use the same license.

**Size:** 1,259 responses, 27 columns. Each row is one respondent. The columns cover demographics (age, gender, country), employer details (company size, remote work, benefits), and attitudes about discussing mental health at work.

**Outcome of interest:** `treatment`, which answers "Have you sought treatment for a mental health condition?" It is close to evenly split: 637 Yes and 622 No.

### Cleaning summary

The full reasoning for every decision is in `notebooks/01_data_cleaning.ipynb`. No respondents were dropped.

| Column | Problem | Fix |
|---|---|---|
| `Age` | 8 invalid values (negative, 5–11, 329, 99999999999) | Replaced with the median (31) |
| `Gender` | Free text with ~40 variants and typos | Grouped into Male / Female / Non-binary/Other / Unknown; originals kept in `Gender_raw` |
| `state` | Blank for non-US respondents; 4 non-US rows had a state anyway | "Non-US" for non-US rows, "Unknown" for 11 US blanks |
| `Country` | Long tail of countries with very few respondents | Kept as is; added `Country_grouped` (countries with fewer than 20 respondents become "Other") |
| `self_employed` | 18 blanks, all in the first minutes of the survey | Filled with the most common answer, "No" |
| `work_interfere` | 264 blanks, from respondents without a condition | Filled with "Not applicable" |
| `comments` | 87% empty optional free text | Dropped (still in the raw file) |
| `Timestamp` | Stored as text; 69 responses came in 2015–2016 | Converted to datetime; late responses kept |

**Result:** 1,259 rows and 28 columns, with no missing values or duplicates.

### Notes for the next stages

- **Leakage risk:** `work_interfere` asks how a mental health condition affects work, so it nearly gives away `treatment`. Including it as a model feature will inflate accuracy. Decide deliberately whether to use it, and report results both with and without it.
- **Small groups:** Non-binary/Other has only 14 respondents, which is too few for reliable statistical tests or subgroup metrics on its own. For country, use `Country_grouped` in analysis and modeling. The original `Country` column still has many countries with very few respondents.
- **Ordered scales:** `no_employees`, `leave`, and `work_interfere` have a natural order, but a CSV can't store it, so they'll sort alphabetically unless you set the order after loading:

  ```python
  size_order = ["1-5", "6-25", "26-100", "100-500", "500-1000", "More than 1000"]
  df["no_employees"] = pd.Categorical(df["no_employees"], categories=size_order, ordered=True)
  ```
