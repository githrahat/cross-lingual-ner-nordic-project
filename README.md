A CUDA-capable GPU is strongly recommended for the NER fine-tuning
notebooks. CPU runs are possible but slow (~30–60 minutes per model
vs.\ ~3–5 minutes on GPU).

### Data sources

| Dataset | Where located |
|---|---|
| English EWT-UD (NER) | `en_ewt-ud-*.iob2` files in repo root |
| WikiANN (DA/SV/NO NER) | Auto-downloaded via HuggingFace `datasets` |
| OPUS bible-uedin | `{English,Danish,Swedish,Norwegian}.xml` in repo root |

## Reproducing the results

Run the notebooks in numerical order. Each notebook produces
artefacts (trained models, evaluation results, figures) that later
notebooks may use.

1. **`01_ner_baseline_english.ipynb`** — fine-tunes mBERT on English
   EWT-UD. Saves the model to `./ner_model_output/`. Expected
   runtime: ~10 min on GPU.

2. **`02_ner_scandinavian_wikiann.ipynb`** — fine-tunes mBERT on
   Danish, Swedish, and Norwegian WikiANN separately. Saves three
   models to `./ner_model_output_{da,sv,no}/`. Expected runtime:
   ~30 min per language on GPU.

3. **`03_ner_crosslingual_eval.ipynb`** — loads the English model
   from step 1 and evaluates it zero-shot on the three Scandinavian
   WikiANN test splits. Expected runtime: ~5 min on GPU.

4. **`04_cosine_similarity_bible.ipynb`** — runs both Case 1
   (per-language fine-tuning) and Case 2 (English-only fine-tuning)
   experiments using the four Bible XML files. Saves five fine-tuned
   models and produces the comparison figures. Expected runtime:
   ~15 min on GPU.

### file paths

The notebooks were originally developed with absolute paths on ours
machines. Before running, checkthe cells at the top of each notebook
and update any hardcoded paths to point to the files in this
repository's root, or to your local working directory.

## Hyperparameters

All NER experiments use the same hyperparameters: mBERT
(`bert-base-multilingual-cased`), learning rate 2e-5, 3 epochs, train
batch size 8, max sequence length 512, fp16 mixed precision, seed 42.

All cosine similarity experiments use the same hyperparameters:
`paraphrase-multilingual-mpnet-base-v2`, `MultipleNegativesRankingLoss`,
3 epochs, batch size 32, 10% warmup, seed 42.

See Tables 3 and 4 of the report for the full hyperparameter
specification, or the configuration cells at the top of each notebook.

## Trained model weights

Fine-tuned model weights (~700 MB each) are produced as a side effect
of running the notebooks and are not stored in this repository due to
size limits. Unfortunately, we were not albo to solve it ourselves. 


