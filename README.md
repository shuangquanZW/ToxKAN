# ToxKAN

ToxKAN is a knowledge-guided Kolmogorov--Arnold Network framework for mechanistic drug toxicity prediction. It integrates chemical structure features with a biologically constrained transcriptomic hierarchy to support both predictive modeling and mechanism-oriented interpretation.

> **Naming update**
> The project was previously developed under the name **KAVNN**. In the manuscript and current presentation, **KAVNN has been renamed to ToxKAN**. Some source files, classes, and scripts in this code snapshot still use the legacy `KAVNN` / `KA_VNN` naming convention for backward compatibility. In this repository:
>
> - **ToxKAN** = current project name used in the paper
> - **KAVNN / KA_VNN** = legacy code identifiers referring to the same model family

## Overview

ToxKAN is designed for toxicity prediction from matched:

- **Transcriptomic profiles**
- **Compound structural features** (Morgan fingerprints)
- **Biologically structured prior knowledge** linking genes to higher-order toxicological entities

The framework combines:

1. a transcriptomic branch with visible hierarchical propagation,
2. a chemical branch for compound encoding, and
3. a fusion predictor for binary and multi-label toxicity tasks.

The current codebase includes training code for:

- **Binary toxicity prediction**
- **Multi-label pathological phenotype prediction**
- **Attribution / hierarchical interpretation utilities**
- **Embedding and variant experiments**

## Code structure

The main files in this snapshot are:

- `KAVNN_Binary.py`  
  Binary classification model definition and Lightning data module. Although the filename uses the legacy name, this corresponds to the **binary ToxKAN** implementation. fileciteturn2file3

- `KAVNN_Multilabel.py`  
  Multi-label classification model definition and Lightning data module for pathological phenotype prediction. This is the **multi-label ToxKAN** implementation under the old naming convention. fileciteturn2file5

- `KAVNNLayers.py`  
  Core model layers, including the biologically structured hierarchy and fusion predictor. The central class `KAVNNLayer` is the legacy implementation name for the ToxKAN backbone. fileciteturn2file8turn2file6

- `FourierKAN.py`  
  Fourier-based KAN layers, including graph-aware variants used in the biological hierarchy. fileciteturn2file1

- `Loss.py`  
  Loss definitions, including the main `KAVNNLoss` used for optimization. fileciteturn2file9

- `Attribution.py` and `KANHierAttribution.py`  
  Utilities for extracting intermediate representations and propagating relevance through the hierarchy for interpretation analyses. fileciteturn2file0turn2file2

- `kavnn_binary.py`  
  Script entry point for training/testing the binary version of the model. This should be interpreted as a legacy script for **binary ToxKAN**. fileciteturn2file10

- `kavnn_multilabel.py`  
  Script entry point for training/testing the multi-label version of the model. This should be interpreted as a legacy script for **multi-label ToxKAN**. fileciteturn2file11

- `KAVNN_Embedding.py`, `KAVNN_Var.py`, `KAVNN_Var_Test.py`  
  Additional scripts for embedding analysis, architectural variants, and related experiments.

## Model mapping: legacy names to current names

For clarity, the following correspondences may be useful when reading the code and manuscript together:

| Legacy code name | Current paper name |
|---|---|
| `KAVNN` | `ToxKAN` |
| `KA_VNN` | `ToxKAN` model instance |
| `KAVNNLayer` | ToxKAN backbone |
| `kavnn_binary.py` | binary ToxKAN training script |
| `kavnn_multilabel.py` | multi-label ToxKAN training script |

## Expected data layout

The training scripts expect a local `data/` directory containing preprocessed NumPy arrays and edge-index files. Based on the current scripts, the expected files include items such as:

- `expr.npy`
- `compound.npy`
- `label.npy`
- `multi_label.npy`
- organ-specific index / label files
- DrugMatrix prediction files
- `edge_index/` files such as GO/KE/tissue connectivity arrays

Examples referenced in the current code include:

- `data/expr.npy`
- `data/compound.npy`
- `data/label.npy`
- `data/multi_label.npy`
- `data/edge_index/gene_go_bp.npy`
- `data/edge_index/go_ke_bp.npy`
- `data/edge_index/ke_ke.npy`
- `data/edge_index/tissue.npy` fileciteturn2file3turn2file5turn2file10turn2file11

Because this repository snapshot contains code files only, dataset generation / preprocessing scripts may need to be added separately or documented in a future revision.

## Environment

The code uses the following core dependencies:

- Python
- PyTorch
- PyTorch Lightning
- torchmetrics
- torch-scatter
- NumPy
- pandas
- scikit-learn
- transformers
- shap

A typical environment setup may therefore look like:

```bash
pip install torch torchvision torchaudio
pip install pytorch-lightning torchmetrics torch-scatter
pip install numpy pandas scikit-learn transformers shap
```

You may need to install `torch-scatter` using a wheel compatible with your PyTorch and CUDA versions.

## Usage

### 1. Binary toxicity prediction

Run the legacy binary script:

```bash
python kavnn_binary.py
```

This launches the binary ToxKAN training/testing pipeline using default arguments defined in the script. The default configuration includes values such as:

- `num_go = 28539`
- `num_ke = 1381`
- `batch_size = 32`
- `max_epochs = 50`
- GPU acceleration by default when available in the script settings. fileciteturn2file10turn2file3

### 2. Multi-label pathological phenotype prediction

Run the legacy multi-label script:

```bash
python kavnn_multilabel.py
```

This launches the multi-label ToxKAN pipeline for 19 pathological phenotype labels by default. fileciteturn2file11turn2file5

### 3. Organ-specific experiments

Both binary and multi-label modules include support for optional `organ` settings such as `liver` and `kidney` in their data modules and helper functions. This is used for organ-specific training / evaluation splits in the current code. fileciteturn2file3turn2file5

## Notes on interpretation utilities

The repository includes code for mechanism-oriented interpretation, including:

- extraction of hidden states,
- surrogate attribution workflows,
- hierarchical relevance propagation across biological layers.

These utilities support the mechanistic interpretation analyses reported for the ToxKAN framework, although additional driver scripts may be needed depending on how the experiments are organized locally. fileciteturn2file0turn2file2

## Suggested future cleanup

Since the project name has changed from **KAVNN** to **ToxKAN**, a future code cleanup could rename the following for consistency:

- `KAVNN_Binary.py` → `ToxKAN_Binary.py`
- `KAVNN_Multilabel.py` → `ToxKAN_Multilabel.py`
- `KAVNNLayers.py` → `ToxKANLayers.py`
- `kavnn_binary.py` → `toxkan_binary.py`
- `kavnn_multilabel.py` → `toxkan_multilabel.py`
- internal classes such as `KAVNNLayer` / `KA_VNN` → `ToxKANLayer` / `ToxKAN`

If you make these changes, remember to update import statements throughout the project.

## Data availability

All raw data utilized in this study are publicly accessible. The Open TG-GATES dataset can be retrieved from [Open TG-GATES](https://dbarchive.biosciencedbc.jp/en/open-tggates/download.html), while the external validation dataset DrugMatrix is available from [DrugMatrix](https://ntp.niehs.nih.gov/data/drugmatrix). The processed datasets are also publicly available on [Zenodo](https://zenodo.org/records/18491821).

## Citation

If you use this codebase, please cite the corresponding ToxKAN manuscript.

## Disclaimer

This README is written for the current code snapshot. Some filenames, imports, and data assumptions still reflect the older internal project naming and local experimental environment.
