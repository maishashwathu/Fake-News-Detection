# Running the original notebook

The uploaded notebook is preserved unchanged. It contains saved results, but a fresh kernel needs the adjustments below. Work on a local copy if you want to retain the original output record.

## 1. Install dependencies

From the repository root, in an activated Python environment:

```bash
python -m pip install -r requirements.txt
jupyter lab Fake-News-Detection.ipynb
```

Dependencies are bounded setup recommendations, not an exact training environment. No fresh full training run has been validated for this documentation.

## 2. Add the missing imports

Insert this cell before the dataset loading and exploration cells:

```python
from pathlib import Path
import re
import string

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from wordcloud import WordCloud

from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report
```

The original notebook uses these names without importing most of them.

## 3. Configure local data paths

Skip the Google Drive mounting cell. Replace the two CSV-loading statements with:

```python
DATA_DIR = Path("data")
df_fake = pd.read_csv(DATA_DIR / "Fake.csv", low_memory=False)
df_true = pd.read_csv(DATA_DIR / "True.csv", low_memory=False)
```

Launch Jupyter from the repository root. See [the data guide](../data/README.md) for the schema. Investigate missing or malformed text before cleaning or generating word clouds; the historical outputs show no null values in the modeling dataframe, but your files may differ.

In Colab, keep the Drive mounting cell, import `pandas` first, and point the original paths to your own files. Do not add `google-colab` as a local dependency.

## 4. Skip the opening BERT experiment

The first four code cells install/load BERT, define an embedding function, and access `df["text"]` before `df` exists. Skip them for the demonstrated TF-IDF workflow.

If exploring BERT separately, install `requirements-bert.txt`, prepare the dataframe first, and call `bert_model.eval()` before inference. The original code processes one article at a time on CPU, truncates to 512 tokens, and extracts the CLS embedding. It requires an initial model download. Assigning embeddings to `X` does not connect them to the later models, which explicitly train on `xv_train`; a separate split, classifier, and evaluation are needed for a BERT result.

## 5. Handle the manual CSV export

Keep the dataframe concatenation and `to_csv` call. Skip `files.download("manual_testing.csv")` locally; the CSV is already written to the current directory.

For Colab downloads, add `from google.colab import files` first.

## 6. Execute the TF-IDF workflow

Run from dataset loading through the final manual prediction cell, applying the changes above. The last cell waits for text input. Linear-kernel `SVC` can take significant time on the full dataset.

For repeatable future experiments, replace:

```python
df = df.sample(frac=1)
```

with:

```python
df = df.sample(frac=1, random_state=42)
```

This creates a reproducible new shuffle; it does not recreate the historical split. The original split is not stratified. Adding stratification or removing duplicates changes the experiment and should be reported with newly computed metrics.

## Artifacts and verification

The notebook exports manual examples, but does not save fitted models or the TF-IDF vectorizer. Manual prediction works only after training in the same session. Save the fitted vectorizer alongside any classifier before implementing standalone inference.

The metrics in the README and results report were checked against the existing notebook outputs. Dataset access, dependency installation, and end-to-end model training were not verified.
