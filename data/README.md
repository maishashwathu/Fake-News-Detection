# Dataset setup

Place `Fake.csv` and `True.csv` in this folder. They are intentionally ignored by Git.

| Column | Expected content |
| --- | --- |
| `title` | Article headline |
| `text` | Article body; used by the classifiers |
| `subject` | Source category; removed before modeling |
| `date` | Publication date; removed before modeling |

The notebook assigns class 0 to fake articles and class 1 to true articles. Labels do not need to be supplied in the input CSVs.

The saved notebook loaded 23,502 fake and 21,417 true rows. The fake CSV had extra `Unnamed` columns that the notebook removed. Different source files or parsing settings may produce different counts and results.

The source URL, dataset version, and license are not documented in the uploaded notebook. Verify these for your copy. Do not commit downloaded datasets without checking redistribution terms.

See [execution instructions](../docs/REPRODUCIBILITY.md) for the required path changes.
