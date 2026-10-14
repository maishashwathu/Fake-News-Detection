# Results extracted from the uploaded notebook

Source: [Fake-News-Detection.ipynb](../Fake-News-Detection.ipynb), original upload commit `da7f99172f1c05905e0a3e94d0aed9853c09db08`.

These are historical saved outputs. No models were retrained to produce this report.

## Accuracy

| Model | Accuracy as saved | Percentage |
| --- | ---: | ---: |
| Logistic Regression | 0.9875278396436525 | 98.7528% |
| Random Forest | 0.9898440979955456 | 98.9844% |
| Linear SVM | 0.9942984409799555 | 99.4298% |

## Classification reports

Values below retain the notebook's two-decimal rounding.

| Model | Class | Precision | Recall | F1 | Support |
| --- | --- | ---: | ---: | ---: | ---: |
| Logistic Regression | 0 — Fake | 0.99 | 0.99 | 0.99 | 5,861 |
| Logistic Regression | 1 — True | 0.99 | 0.99 | 0.99 | 5,364 |
| Random Forest | 0 — Fake | 0.99 | 0.99 | 0.99 | 5,861 |
| Random Forest | 1 — True | 0.99 | 0.99 | 0.99 | 5,364 |
| Linear SVM | 0 — Fake | 1.00 | 0.99 | 0.99 | 5,861 |
| Linear SVM | 1 — True | 0.99 | 0.99 | 0.99 | 5,364 |

All three saved reports show macro and weighted precision, recall, and F1 of 0.99 after rounding.

## Experiment context

- Original data: 23,502 fake + 21,417 true = 44,919 articles.
- Manual examples reserved: final 10 rows from each CSV, for 20 total.
- Modeling dataset: 44,899 articles.
- Split: `test_size=0.25, random_state=42`; 33,674 training and 11,225 test rows.
- An earlier `df.sample(frac=1)` has no random seed.
- Features: cleaned article bodies with default `TfidfVectorizer()`; fitted on training data.
- Estimators: default `LogisticRegression()`, `RandomForestClassifier(random_state=0)`, and `SVC(kernel='linear', random_state=0)`.
- Saved visualizations: word clouds for fake and true article text.
- BERT cells have no saved evaluation and do not supply the features for these classifiers.

## Interpretation

The linear SVM has the highest recorded accuracy on this split. The notebook does not establish performance on unseen publishers, newer articles, other languages, or independently verified claims. Duplicate and source leakage were not audited. Package versions and dataset provenance were not recorded, and the unseeded shuffle prevents exact split recovery.
