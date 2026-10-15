# Adversarial Attacks Reveal Divergent Deception Competence in Humans and Generative Language Models

[![arXiv](https://img.shields.io/badge/arXiv-2610.13363-b31b1b.svg)](https://arxiv.org/abs/2610.13363)

Code and data for the computational text analyses reported in:

> Pfründer, L., Loconte, R., Verschuere, B., & Kleinberg, B. (2026). Adversarial attacks reveal divergent deception competence in humans and generative language models. *arXiv preprint arXiv:2610.13363*. https://doi.org/10.48550/arXiv.2610.13363

## About

The repository contains the Python analysis pipeline behind these results:

1. **Linguistic features** — Scoring 30 features per original and modified statements (26 DeCLaRatiVE deception features plus four part-of-speech features). This was used to investigate how language models and humans modified statements
2. **Strategy labeling** — BERTopic topic modeling of participants' self-reported attack strategies, followed by assigning strategy codes (authenticity, emotion, rephrasing, details).
3. **Classifier training** — logistic regression, random forest, gradient boosting, and SVM classifiers trained to distinguish original from modified statements using the 30 linguistic features.

All other analyses reported in the manuscript were conducted in R and are not part of this repository.

## Repository structure

```text
├── data/
│   ├── combined_best_attempt.csv        # Best attempt per attack sequence, incl. self-reported strategy
│   └── combined_long_liwc.csv           # All texts (original + modification) with LIWC-22 scores
├── linguisticFeatures/
│   ├── linguistic_features.ipynb                  # Feature extraction and statistical comparison
│   ├── Brysbaert concreteness scoring.xlsx        # Concreteness ratings (Brysbaert et al., 2014)
│   └── Concreteness ratings for 40 thousand generally known English word lemmas.pdf
├── strategyLabeling/
│   └── strategy_labeling.ipynb           # Strategy topic modeling and coding
├── classifierTraining/
│   └── classifier_training.ipynb         # Classifier training and evaluation
├── output/                              # Where every notebook stores its results
├── environment.yml                      # Conda environment definition
├── LICENSE
└── README.md
```

## Setup

Requires [Miniconda](https://docs.conda.io/en/latest/miniconda.html) (or a full Anaconda installation).

```bash
conda env create -f environment.yml
conda activate deception-competence
python -m spacy download en_core_web_trf   # required for Stage 1
```

Optional — register a Jupyter kernel:

```bash
python -m ipykernel install --user --name deception-competence --display-name "Python (deception-competence)"
```

The environment provides Python 3.11 with pandas, spaCy, textstat, pingouin, BERTopic, and scikit-learn (see `environment.yml` for the full list).

## Reproducing the analyses

1 and 2 are independent; 3 requires the output of 1.

| Stage | Notebook | Input | Main outputs |
| --- | --- | --- | --- |
| 1. Linguistic features | `linguisticFeatures/linguistic_features.ipynb` | `data/combined_long_liwc.csv`, concreteness ratings | `output/combined_long_lingfeat.csv`, `output/combined_long_lingfeat_linked.csv` |
| 2. Strategy labeling | `strategyLabeling/strategy_labeling.ipynb` | `data/combined_best_attempt.csv` | `output/combined_best_attempt_strategy.csv`, BERTopic topic-info tables |
| 3. Classifier training | `classifierTraining/classifier_training.ipynb` | `output/combined_long_lingfeat_linked.csv` | `output/classifier_original_vs_modified.csv` |

All generated output is written to `output/`.

### Reproducibility notes

- 3 fixes `SEED = 42` for the grouped train/test split, cross-validation, classifier training, and permutation importance.
- To prevent leakage, the classifier evaluation splits the data by `statement_id`: the original statement and all of its modifications always fall on the same side of the split.
- BERTopic (2) uses UMAP dimensionality reduction, which is stochastic, so exact topic assignments can vary slightly between runs. The final strategy labels (2, 5) were assigned after inspecting the top words and representative documents of each topic. For this part, the output of the notebook is saved.

## Data

- `data/combined_best_attempt.csv` — one row per attack sequence (best attempt): participant demographics, statement and attack metadata, and the self-reported `strategies` text analyzed in Stage 2.
- `data/combined_long_liwc.csv` — one row per text: the original statement and the best modification of every attack sequence, with identifiers (`rewrite_id`, `statement_id`, `attack_modality`, `text_type`) and LIWC-22 scores (Boyd et al., 2022) used in Stage 1.
- `linguisticFeatures/Brysbaert concreteness scoring.xlsx` — concreteness ratings for ~40,000 English word lemmas, used for the concreteness feature (Brysbaert, Warriner, & Kuperman, 2014).
Concreteness reference:

> Brysbaert, M., Warriner, A. B. & Kuperman, V. Concreteness ratings for 40 thousand generally known English word lemmas. Behav Res 46, 904–911 (2014).

LIWC-22 reference: 

> Boyd, R. L., Ashwini Ashokkumar, Seraj, S. & Pennebaker, J. W. The Development and Psychometric Properties of LIWC-22. https://doi.org/10.13140/RG.2.2.23890.43205 (2022) doi:10.13140/RG.2.2.23890.43205.


## Citation

If you use this code or data, please cite:

```bibtex
@misc{pfrunder2026adversarial,
  author        = {Pfr{\"u}nder, Lucca and Loconte, Riccardo and Verschuere, Bruno and Kleinberg, Bennett},
  title         = {Adversarial Attacks Reveal Divergent Deception Competence in Humans and Generative Language Models},
  year          = {2026},
  eprint        = {2610.13363},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CY},
  doi           = {10.48550/arXiv.2610.13363},
  url           = {https://arxiv.org/abs/2610.13363}
}
```

## License

This repository is released under the [Creative Commons Attribution 4.0 International License (CC-BY 4.0)](LICENSE), matching the license of the preprint.

## Contact

For questions or issues, contact l.j.pfruender@tilburguniversity.edu
