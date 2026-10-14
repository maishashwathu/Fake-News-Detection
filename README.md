# Fake News Detection

Classifying news article text with **TF-IDF** and three supervised machine learning models: Logistic Regression, Random Forest, and a linear Support Vector Machine.

[Explore the notebook](Fake-News-Detection.ipynb) · [Run instructions](docs/REPRODUCIBILITY.md) · [Recorded results](reports/notebook-results.md)

## Project overview

This project explores a complete text classification workflow: inspecting two news datasets, assigning binary labels, cleaning article text, extracting TF-IDF features, comparing classifiers, and testing predictions on manually entered text.

- **Input:** article body in the `text` column; titles are retained during preparation but are not used as model features.
- **Labels:** `0` = Fake News; `1` = Not A Fake News (the source dataset's true class).
- **Models:** scikit-learn Logistic Regression, Random Forest, and `SVC(kernel="linear")`.
- **Evaluation:** accuracy and per-class precision, recall, and F1.

## Recorded results

These values come from existing notebook outputs, **not a fresh training run**.

| Model | Recorded test accuracy |
| --- | ---: |
| Logistic Regression | 98.75% |
| Random Forest | 98.98% |
| Linear SVM | **99.43%** |

The saved reports contain **11,225 test articles**: 5,861 fake and 5,364 true. The notebook uses a 75/25 train/test split with `random_state=42`, but its earlier dataframe shuffle has no seed, so the original split cannot be reconstructed exactly. See [results and limitations](reports/notebook-results.md).

## Workflow

1. Read `Fake.csv` and `True.csv`; remove extra `Unnamed` columns from the fake dataset.
2. Label both datasets and reserve the final 10 rows from each for manual testing.
3. Merge, shuffle, reset the index, and clean the article body using regular expressions.
4. Split text and labels into training and test sets.
5. Fit TF-IDF on training text only, then transform the test text.
6. Train and evaluate all three classifiers.
7. Transform a new article using the fitted vectorizer and display each model's prediction.

The opening BERT cells are an **unfinished embedding experiment**. They reference `df` before it is created and are not connected to the evaluated TF-IDF classifiers. This repository does not demonstrate BERT fine-tuning or a measured BERT result.

## Getting started

Use Python 3.10 or 3.11 as a suggested starting environment; the original environment was not recorded.

```bash
git clone https://github.com/maishashwathu/Fake-News-Detection.git
cd Fake-News-Detection
python -m venv .venv
```

Activate the environment:

```powershell
# Windows PowerShell
.venv\Scripts\Activate.ps1
```

```bash
# macOS / Linux
source .venv/bin/activate
```

Then install dependencies and open Jupyter:

```bash
python -m pip install -r requirements.txt
jupyter lab Fake-News-Detection.ipynb
```

**Before executing:** follow [the reproducibility guide](docs/REPRODUCIBILITY.md) to add the missing imports, set dataset paths, and skip Colab-only and BERT cells locally. The original notebook is preserved as uploaded and is not ready for an unmodified “Run All.”

## Data

Provide your own `Fake.csv` and `True.csv`, each with `title`, `text`, `subject`, and `date` columns. Suggested location: `data/`. The recorded input sizes are 23,502 fake and 21,417 true rows; 44,899 rows remain after reserving 20 manual examples.

The notebook does not identify an authoritative dataset download URL or license. Confirm the source, version, and reuse terms for your copy before redistribution. No datasets or trained model files are included.

## Repository contents

| Path | Purpose |
| --- | --- |
| `Fake-News-Detection.ipynb` | Original experiments, saved reports, word clouds, and interactive predictions |
| `requirements.txt` | Dependencies for the TF-IDF workflow and notebook |
| `requirements-bert.txt` | Optional dependencies for the unfinished BERT experiment |
| `docs/REPRODUCIBILITY.md` | Execution fixes, local/Colab setup, and reproducibility notes |
| `reports/notebook-results.md` | Extracted metrics and evaluation context |
| `data/README.md` | Expected input files and schema |

## Limitations and next steps

The evaluation uses one random article split, with no demonstrated duplicate removal, source-separated evaluation, temporal holdout, or external test set. Publisher wording and repeated articles may inflate apparent performance. Dataset labels are not independent verification of a new article's claims.

Useful next steps include recording dataset provenance, removing duplicates before splitting, seeding the shuffle, evaluating on newer sources, and exporting each model together with its fitted vectorizer. No deployment or pretrained inference artifact is currently included.

This is an educational text classification project; predictions should be interpreted as dataset-based classifications.
