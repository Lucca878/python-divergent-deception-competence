# analysesStudy1Python

Python workspace for RQ2 focused on data preprocessing and NLP feature generation.

Statistical analyses are performed in R; this repository prepares clean, documented artifacts for that stage.

## Project goals

- Ingest and clean study datasets.
- Build reproducible NLP preprocessing pipelines.
- Generate analysis-ready outputs to be consumed by R scripts.
- Keep a clear separation between source data, transformation logic, and generated outputs.

## Repository structure

- `data/`: input datasets used for preprocessing.
- `linguisticFeatures/`: notebooks and resources for linguistic feature extraction.
- `notebooks/`: exploratory notebooks for method checks and prototyping.
- `output/`: generated artifacts (ignored by git).
- `environment.yml`: reproducible Conda environment definition.

## Environment setup (Miniconda)

1. Install Miniconda (if needed): https://docs.conda.io/en/latest/miniconda.html
2. From this repository root, create the environment:

```bash
conda env create -f environment.yml
```

If the environment already exists and `environment.yml` changed:

```bash
conda env update -f environment.yml --prune
```

3. Activate it:

```bash
conda activate rq2-nlp
```

4. Optional: register kernel for notebooks:

```bash
python -m ipykernel install --user --name rq2-nlp --display-name "Python (rq2-nlp)"
```

5. Install a spaCy English model (required for linguistic feature notebooks):

```bash
python -m spacy download en_core_web_trf
```

## Workflow

1. Keep datasets in `data/` with clear naming.
2. Use notebooks or scripts as needed for preprocessing/NLP steps.
3. Write generated files to `output/`.
4. Export analysis-ready files for downstream R analyses.

## Linguistic Feature Pipeline

Notebook-only implementation for combined LIWC/text features is available in:

- `linguisticFeatures/combined_long_liwc_lingfeat.ipynb`

This notebook:

- reads `data/combined_long_liwc.csv`
- preserves the original feature logic from prior notebooks
- adapts grouping to `text_type` (`original` vs `rewrite`)
- adds spaCy POS-tag percentages for adjectives, nouns, verbs, and adverbs
- writes the final feature table to `output/combined_long_liwc_lingfeat.csv`

## Reproducibility notes

- Keep raw data immutable.
- Record major preprocessing decisions in your analysis notes/notebooks.
- Avoid committing large generated files unless needed for a publication snapshot.

## Status

Minimal scaffold completed.
