# English → Urdu Word Meaning Dataset

A flat, ready-to-use lexicon of **100,000 English words paired with their Urdu meanings**, distributed as a single CSV file. Built for NLP pipelines, dictionary apps, language-learning tools, and translation model training/evaluation.

## 📦 Dataset at a Glance

| | |
|---|---|
| **File** | `eng_to_urdu.csv` |
| **Format** | CSV (UTF-8 with BOM) |
| **Rows** | 100,000 word–meaning pairs |
| **Columns** | 2 (`word`, `meaning`) |
| **Size** | ~2.1 MB |
| **Language pair** | English (en) → Urdu (ur) |

## 🗂️ Schema

| Column | Type | Description |
|---|---|---|
| `word` | string | English word or short term |
| `meaning` | string | Corresponding Urdu meaning/translation |

**Sample rows:**

| word | meaning |
|---|---|
| more | مزید |
| to | کو |
| and | اور |
| is | ہے |
| with | کے ساتھ |

## 🚀 Quick Start

### Python (pandas)
```python
import pandas as pd

df = pd.read_csv("eng_to_urdu.csv", encoding="utf-8-sig")
df.columns = ["word", "meaning"]

print(df.head())
print(f"Total entries: {len(df):,}")

# Look up a word
meaning = df[df["word"].str.lower() == "book"]["meaning"].values
print(meaning)
```

### Python (dict lookup)
```python
import csv

lookup = {}
with open("eng_to_urdu.csv", encoding="utf-8-sig") as f:
    reader = csv.reader(f)
    next(reader)  # skip header
    for word, meaning in reader:
        lookup[word.strip().lower()] = meaning.strip()

print(lookup.get("water"))
```

### JavaScript (Node.js)
```javascript
const fs = require("fs");
const { parse } = require("csv-parse/sync");

const raw = fs.readFileSync("eng_to_urdu.csv", "utf-8");
const records = parse(raw, { columns: true, skip_empty_lines: true });

console.log(records.slice(0, 5));
```

## ✨ Use Cases

- Building **English–Urdu dictionary** apps or browser extensions
- **Vocabulary/flashcard** tools for language learners
- Seed data or evaluation set for **machine translation** models
- **NLP research** on low-resource South Asian languages
- Autocomplete/spell-check glossaries for Urdu-language software

## ⚠️ Known Data Notes

- A small number of rows (~0.3%) have missing or malformed entries — filter these out before use:
  ```python
  df = df.dropna(subset=["word", "meaning"])
  df = df[df["word"].str.strip() != ""]
  ```
- Meanings are single/short-form translations, not full dictionary definitions (no part-of-speech, example sentences, or multiple senses per word).
- File is UTF-8 with a BOM — use `encoding="utf-8-sig"` in Python to avoid a stray character on the first column name.

## 🛠️ Suggested Cleaning Steps

```python
import pandas as pd

df = pd.read_csv("eng_to_urdu.csv", encoding="utf-8-sig")
df.columns = ["word", "meaning"]
df = df.dropna(subset=["word", "meaning"])
df = df[(df["word"].str.strip() != "") & (df["meaning"].str.strip() != "")]
df["word"] = df["word"].str.strip().str.lower()
df["meaning"] = df["meaning"].str.strip()
df = df.drop_duplicates(subset="word")

df.to_csv("eng_to_urdu_clean.csv", index=False, encoding="utf-8-sig")
```

## 📄 License

This dataset is released under **[CC0 1.0 Universal (Public Domain Dedication)](https://creativecommons.org/publicdomain/zero/1.0/)**.
 
You can copy, modify, distribute, and use this dataset — including for commercial purposes — without asking permission or providing attribution.

## 🤝 Contributing

Corrections, missing-word additions, and translation-quality fixes are welcome — open an issue or submit a pull request with the corrected `word,meaning` rows.