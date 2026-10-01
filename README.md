# PeruSAT-1 Building Footprint Segmentation

Companion repository for the paper **Deep Learning-Based Building Footprint Segmentation Across Heterogeneous Peruvian Landscapes Using PeruSAT-1 Imagery**.

The repository contains notebooks and derived public artifacts for territorial stratification, dataset description, and building segmentation with **U-Net ResNet-50, DeepLabV3+ ResNet-50, and SegFormer MiT-B2**. All three models use **RGB+NIR (four bands)** with ImageNet-pretrained encoders and a shared training and pixel-level evaluation protocol.

## Repository Structure

```text
1) DATA AND TERRITORIAL STRATIFICATION/
  embeddings.ipynb
  clusters_id.geojson
  Data/
  Outputs/

2) ANNOTATION AND DATASET PREPARATION/
  region_stats_edificios.gpkg
  provincia_areas_buildings.html

3) MODEL ARCHITECTURE AND TRAINING/
  BuildingsPeruSat1.ipynb

README.md
requirements.txt
environment.yml
```

## What Is Included

- Territorial stratification notebook and its saved outputs.
- Public geospatial layers used for descriptive analysis.
- Interactive and static maps used to document dataset coverage.
- A shared notebook for training, validation, testing, and qualitative comparison of the three architectures.
- RGB ImageNet normalization and NIR statistics computed exclusively from valid training pixels, reused for all three models.
- Pixel-level precision, recall, F1, F0.5, and IoU accumulated with TorchMetrics over valid pixels, using a fixed prediction threshold of `probability > 0.5`.

## What Is Not Included

The repository does not distribute private or restricted inputs:

- Raw PeruSat-1 imagery.
- Pixel-level masks used to train the segmentation models.
- Private archives, model checkpoints, and exported model weights.
- Internal code used to produce the private image/mask training dataset.

## Environment

The local environment is defined in `environment.yml`, which installs the shared dependencies from `requirements.txt`. Run these commands from the repository root:

```bash
conda env create -f environment.yml
conda activate perusat1-building-footprint-segmentation
python -m ipykernel install --user --name perusat1-building-footprint-segmentation --display-name "PeruSAT-1 Building Footprint Segmentation"
```

Alternatively, install the Python dependencies with pip:

```bash
pip install -r requirements.txt
```

Training and evaluation are configured for one CUDA GPU with mixed precision. The notebook's installation cell specifies the exact package versions used by its Colab setup, including PyTorch 2.6.0, torchvision 0.21.0, SMP 0.5.0, Lightning 2.5.1, TorchMetrics 1.7.1, and Albumentations 2.0.8. The repository requirements remain general except for SMP and Albumentations, whose interfaces are used directly by the notebook. They are not a lockfile for reproducing the Colab environment.

For local GPU execution, install a compatible PyTorch/torchvision CUDA build and skip the Colab installation, Drive-mounting, and archive-extraction cells. In Colab, use the notebook's installation cell and restart the runtime if requested before continuing.

## Running the Notebooks

### Territorial Stratification

Open:

```text
1) DATA AND TERRITORIAL STRATIFICATION/embeddings.ipynb
```

This notebook uses the included public geospatial inputs and the restricted populated-centers layer described in [Data/PopulatedCenters/README.md](<1) DATA AND TERRITORIAL STRATIFICATION/Data/PopulatedCenters/README.md>). Supply that layer to rerun the full workflow. Descriptive maps are written under `Outputs/`.

### U-Net, DeepLabV3+, and SegFormer Training and Evaluation

Open:

```text
3) MODEL ARCHITECTURE AND TRAINING/BuildingsPeruSat1.ipynb
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

Set the paths before starting Jupyter:

```bash
export PERUSAT1_DATA_ROOT="/path/to/Dataset"
export PERUSAT1_RUN_DIR="/path/to/buildings_runs"
```

On Windows PowerShell:

```powershell
$env:PERUSAT1_DATA_ROOT="D:\path\to\Dataset"
$env:PERUSAT1_RUN_DIR="D:\path\to\buildings_runs"
```

In Colab, mount Drive and set `PERUSAT1_DATA_ROOT` and `PERUSAT1_RUN_DIR` through `os.environ` before the paths cell, or edit `DATA_ROOT` and `PERSISTENT_ROOT` there. Use a Drive directory for persistent artifacts. Defaults are `/content/Dataset` for data and `runs/` relative to the working directory for artifacts.

Run the setup and shared definitions in order, then the architecture training cells. Training automatically resumes from `<PERSISTENT_ROOT>/<model_name>/checkpoints/last.ckpt` when it exists. Use a new artifact root for a fresh experiment or changed data, band order, scaling, or hyperparameters; the same root also caches `nir_stats.json`.

The best checkpoint is selected by validation F0.5. The subsequent validation and explicit test sections produce `comparison_pixel_metrics.csv` and per-model `pixel_metrics.csv`. Each architecture also exports its best weights to `.pth`; comparative figures are saved under `qualitative_figures/`.

## Data Availability

This repository provides public descriptive geospatial outputs and derived analysis artifacts. Restricted source data and model-training imagery are excluded due to privacy and data-use constraints.

## Citation

If you use this repository, cite the associated paper and this repository.
