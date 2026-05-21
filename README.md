# Cross-Lingual Transfer in Multilingual Models: NER and Sentence Similarity

This repository contains the code and data for our project investigating
cross-lingual transfer in multilingual models, comparing **monolingual
fine-tuning** against **zero-shot cross-lingual transfer from English**
across two tasks: Named Entity Recognition (NER) and sentence-level cosine
similarity, on English, Danish, Swedish, and Norwegian.

📄 **[Full report (PDF)](report/main.pdf)**

## Repository structure

```
notebooks/                    Four self-contained Jupyter notebooks
  01_ner_baseline_english     Fine-tune mBERT on English EWT-UD
  02_ner_scandinavian_wikiann Fine-tune mBERT on DA/SV/NO WikiANN (one model each)
  03_ner_crosslingual_eval    Zero-shot eval of English model on DA/SV/NO
  04_cosine_similarity_bible  Case 1 / Case 2 cosine similarity experiments
requirements.txt              Python dependencies
```

## Setup

### 1. Python environment

Tested on Python 3.10+. Install dependencies:

```bash
pip install -r requirements.txt
```

need GPU otherwise HPC

### 2. Data

| Dataset | Source | Setup |
|---|---|---|
| English EWT-UD (NER) | Provided by course or just right here | Place `.iob2` files in `data/ner_data/`. Update `DATA_DIR` path in notebook 01 |
| WikiANN (NER) | Auto-downloaded via HuggingFace `datasets` | 
| OPUS bible-uedin (cosine) | Auto-downloaded from `object.pouta.csc.fi` | 

The English EWT-UD `.iob2` files are here

## Reproducing the results

Run the notebooks **in numerical order**. Each notebook saves its
outputs (trained models, evaluation results, figures) to disk for use
by later notebooks.

### NER pipeline

1. **`01_ner_baseline_english.ipynb`** — fine-tunes mBERT on English
   EWT-UD. Saves the model to `./ner_model_output/`. Expected runtime 15 minutes on HPC or GPU.

2. **`02_ner_scandinavian_wikiann.ipynb`** — fine-tunes mBERT on
   Danish, Swedish, and Norwegian WikiANN separately. Saves three
   models to `./ner_model_output_{da,sv,no}/`. Expected runtime:
   ~30 min on GPU or HPC

3. **`03_ner_crosslingual_evaluation.ipynb`** — loads the English
   model from step 1 and evaluates it zero-shot on the three
   Scandinavian WikiANN test splits. Expected runtime: ~5 min on GPU or HPC

### Cosine similarity pipeline

4. **`04_cosine_similarity_bible.ipynb`** — runs both Case 1
   (per-language fine-tuning) and Case 2 (English-only fine-tuning)
   experiments. Saves five fine-tuned models and produces the
   comparison figures. Expected runtime: ~15 min on GPU.

## Authors

- hotr@itu.dk
- fili@itu.dk
- alam@itu.dk

## Acknowledgements

The Bible corpus was obtained from [OPUS](https://opus.nlpl.eu/) and
the WikiANN dataset via the HuggingFace `datasets` library.
