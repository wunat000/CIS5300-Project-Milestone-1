# Data

## Sources

We use two datasets:

- **HistWords:** [GitHub](https://github.com/williamleif/histwords). Our main dataset contains English word embeddings for each decade from 1800 to 1990. Each decade has vocabulary (`YEAR-vocab.pkl`) and vector (`YEAR-w.npy`) files.
- **SemEval-2020 Task 1 (English):** [Dataset page](https://www.ims.uni-stuttgart.de/en/research/resources/corpora/sem-eval-ulscd-eng/). This includes English text from 1810–1860 and 1960–2010, plus human-annotated semantic-change scores. We keep it separate for possible additional evaluation.

## Processing

We loaded the HistWords embeddings, kept alphabetic words present across all decades, and calculated cosine distance between consecutive decades (`1 - cosine similarity`). We removed words with missing or zero-vector-based scores. This left **12,599 words** with complete trajectories and **239,381** observed change scores across 19 intervals.

We paired each decade's observed change with the preceding decade's change to prepare inputs and targets for later modeling. We have not trained models or made predictions yet.

## CSV files and splits

Each row in `train.csv`, `dev.csv`, or `test.csv` contains:

- `word`: the word
- `start_year`, `end_year`: years of the target interval
- `previous_change`: observed cosine distance in the preceding interval (input)
- `actual_change`: observed cosine distance in the target interval (target)

We split the data chronologically:

| Split | Target intervals | Rows |
|---|---|---:|
| Train | 1810–1820 through 1950–1960 | 188,985 |
| Dev | 1960–1970 | 12,599 |
| Test | 1970–1980 and 1980–1990 | 25,198 |

All splits contain the same 12,599 words, with no missing values or duplicate word–interval rows.

## Files

- `train.csv`, `dev.csv`, `test.csv`: Prepared HistWords data splits.
- `observed_changes.csv`: Individual observed changes for each word and interval.
- `semantic_trajectories.csv`: Observed changes arranged by word across decades.
- `Milestone1.ipynb`: Data preparation code.
- `semeval/`: Original SemEval files.

The original HistWords vectors can be downloaded from the repository linked above.
