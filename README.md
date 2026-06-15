# PeruSAT-1 Building Footprint Segmentation

Companion repository for the paper **Deep Learning-Based Building Footprint Segmentation Across Heterogeneous Peruvian Landscapes Using PeruSAT-1 Imagery**.

The repository contains notebooks and derived public artifacts used for territorial stratification, dataset description, SegFormer model training, evaluation, postprocessing analysis, and qualitative visualization.

## Repository Structure

```text
1) DATA AND TERRITORIAL STRATIFICATION/
  embeddings.ipynb
  Data/
  Outputs/

2) ANNOTATION AND DATASET PREPARATION/
  region_stats_edificios.gpkg
  provincia_areas_buildings.html

3) MODEL ARCHITECTURE AND TRAINING/
  SegFormerPeruSat1.ipynb
```

## What Is Included

- Territorial stratification notebook and its saved outputs.
- Public geospatial layers used for descriptive analysis.
- Interactive and static maps used to document dataset coverage.
- SegFormer training, validation, testing, object-level evaluation, postprocessing search, and qualitative-figure notebook.

## What Is Not Included

The repository does not distribute private or restricted inputs:

- Raw PeruSat-1 imagery.
- Pixel-level masks used to train the segmentation model.
- Private archives, model checkpoints, and exported model weights.
- Internal code used to produce the private image/mask training dataset.

The notebooks keep their outputs to make the paper workflow inspectable without requiring access to the private training data.

## Environment

The recommended environment is defined in `environment.yml`.

```bash
conda env create -f environment.yml
conda activate perusat1-building-footprint-segmentation
python -m ipykernel install --user --name perusat1-building-footprint-segmentation --display-name "PeruSAT-1 Building Footprint Segmentation"
```

Alternatively, install the Python dependencies with pip:

```bash
pip install -r requirements.txt
```

For GPU training, install the PyTorch build that matches your CUDA version before running the SegFormer notebook.

## Running the Notebooks

### Territorial Stratification

Open:

```text
1) DATA AND TERRITORIAL STRATIFICATION/embeddings.ipynb
```

This notebook uses the public geospatial inputs included in the repository and writes descriptive maps under `Outputs/`.

### SegFormer Training and Evaluation

Open:

```text
3) MODEL ARCHITECTURE AND TRAINING/SegFormerPeruSat1.ipynb
```

The private image dataset is not distributed. To rerun the training notebook, prepare the data with this structure:

```text
Dataset/
  train/
    images_norm/
    masks/
  val/
    images_norm/
    masks/
  test/
    images_norm/
    masks/
```

Set the paths before starting Jupyter or Colab:

```bash
export PERUSAT1_DATA_ROOT="/path/to/Dataset"
export PERUSAT1_RUN_DIR="/path/to/segformer_runs"
export PERUSAT1_RESUME_CKPT_PATH="/path/to/checkpoint.ckpt"
export PERUSAT1_BEST_CKPT_PATH="/path/to/best_checkpoint.ckpt"
```

On Windows PowerShell:

```powershell
$env:PERUSAT1_DATA_ROOT="D:\path\to\Dataset"
$env:PERUSAT1_RUN_DIR="D:\path\to\segformer_runs"
$env:PERUSAT1_RESUME_CKPT_PATH="D:\path\to\checkpoint.ckpt"
$env:PERUSAT1_BEST_CKPT_PATH="D:\path\to\best_checkpoint.ckpt"
```

`PERUSAT1_RESUME_CKPT_PATH` is optional. Leave it unset for a fresh training run.

## Data Availability

This repository provides public descriptive geospatial outputs and derived analysis artifacts. Restricted source data and model-training imagery are excluded due to privacy and data-use constraints.

## Citation

If you use this repository, cite the associated paper and this repository.
