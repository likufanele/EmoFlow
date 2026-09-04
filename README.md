# EmoFlow reproducibility package

This archive contains the code and derived numerical results accompanying **EmoFlow: Explicit affective dynamics for narrative emotion forecasting**.
Author: Hiram Calvo

## Contents

- `emoflow_experiments.ipynb`: Notebook containing the frozen-backbone experiment, five-seed BERT-tiny fine-tuning, paired bootstrap tests, per-emotion evaluation, qualitative case extraction, and NRC-VAD coverage analysis.
- `results/frozen_results.csv`: frozen-model results recovered from the completed experiment log and restricted to the five strategies reported in the manuscript.
- `results/finetuning_*.csv`: seed-level and aggregate fine-tuning results used for the manuscript tables and analyses.
- `results/nrc_vad_coverage_reported.csv`: coverage values reported in the manuscript.
- `requirements.txt`: Python dependencies.

The exploratory SenticNet branches and checkpoint-recovery cells in the working notebook were intentionally omitted because they are not part of the reported experimental design. Stored notebook outputs, private Google Drive paths, caches, and model checkpoints were also removed.

## Required data

The source data and NRC-VAD lexicon are not redistributed in this archive. Obtain them from their official providers and comply with their licenses:

- StoryCommonsense: <https://uwnlp.github.io/storycommonsense/>
- NRC Valence, Arousal, and Dominance Lexicon: <https://saifmohammad.com/WebPages/nrc-vad.html>

Place the following files in `data/`:

```text
data/
├── storycs_train.csv
├── storycs_dev.csv
├── storycs_test.csv
└── unigrams-NRC-VAD-Lexicon-v2.1.txt
```

The notebook loads the official StoryCommonsense training file for schema compatibility, but the reported classifiers use the official development partition as the supervised modeling pool and retain the official test partition for evaluation, as described in the manuscript.

## Running the notebook

Create an environment and install the dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab emoflow_experiments.ipynb
```

By default, the notebook reads `data/` and writes caches and results under the current directory. Paths may be overridden before launching Jupyter:

```bash
export EMOFLOW_PROJECT_DIR=/path/to/workdir
export EMOFLOW_DATA_DIR=/path/to/storycommonsense
export NRC_VAD_PATH=/path/to/unigrams-NRC-VAD-Lexicon-v2.1.txt
```

The transformer models are downloaded from Hugging Face on first use. The reported text backbones are `prajjwal1/bert-tiny` and `distilbert-base-uncased`; affective logits are extracted with the fixed `RobroKools/vad-bert` model. Fine-tuning is computationally expensive and produces full checkpoints locally.

## Reproducing the reported analyses

Run the notebook from top to bottom. The frozen section creates the results for both frozen backbones. The fine-tuning section runs the four tasks with seeds `12, 24, 42, 73, 105` and creates the compact, bootstrap, and per-emotion CSV files. The last two sections export qualitative comparisons and NRC-VAD coverage.

The CSV files supplied in `results/` are the completed-run outputs used in the manuscript. `model_path` fields have been reduced to checkpoint filenames so the archive does not expose workstation-specific paths.

## License

The code and derived tables in this archive are released under the Creative Commons Attribution 4.0 International license. StoryCommonsense, NRC-VAD, and pretrained model files remain governed by their respective licenses.
