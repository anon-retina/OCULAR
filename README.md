# OCULAR: Open-source Collection of Unified Large-scale Artery-vein Retinae

OCULAR provides a harmonized large-scale dataset of retinal artery-vein (A/V) annotations and **OCULARNet**, a model trained to prioritize clinically relevant vascular biomarkers over pixel-wise overlap.  

## Table of Contents
- [Dataset](#dataset)  
- [Pre-trained Model](#pre-trained-model)  
- [Installation & Dependencies](#installation--dependencies)  
- [Inference Pipeline](#inference-pipeline)  
- [Example Outputs](#example-outputs)  
- [Training & Evaluation](#training--evaluation)  
- [Zone Extraction](#zone-extraction)
- [Citation](#citation)  

---

## Dataset

OCULAR aggregates a diverse collection of publicly available retinal A/V datasets for **training**, **in-distribution (ID) testing**, and **out-of-distribution (OoD) evaluation**.  
Each block below corresponds to these splits.

**Abbreviations:**  
M = macula-centered, D = optic disc-centered  
FOV = field of view  
DR = diabetic retinopathy, G = glaucoma, HR = hypertensive retinopathy  
POAG = primary open-angle glaucoma, AMD = age-related macular degeneration, H = healthy  

---

### Training Datasets

| Dataset | N | Center | FOV | Resolution (px) | Region | Pathologies | Download |
|--------|---|--------|-----|----------------|--------|-------------| --------|
| AVRDB | 100 | M,D | 45° | 1000×1054 | PK | HR | [[Link]](https://www.researchgate.net/publication/319165214_AVRDB_Annotated_Dataset_for_Vessel_Segmentation_and_Calculation_of_Arteriovenous_Ratio)
| DRIVE | 40 | M | 45° | 584×565 | NL | DR | [[Link]](https://www.kaggle.com/datasets/andrewmvd/drive-digital-retinal-images-for-vessel-extraction)
| ENRICH | 111 | M,D | 45° | 1958×2196 | BE | -- | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| FIVES-AV | 75 | M | 45° | 1444×1444 | CN | -- | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| Fundus-AVSeg | 100 | M | 45° | 1280×1280 | CN | -- | [[Link]](https://figshare.com/projects/Fundus-AVSeg_A_Fundus_Image_Dataset_for_AI-based_Artery-Vein_Vessel_Segmentation/229986)
| GAVE | 50 | M | 45° | 1536×1024 | CN | -- |[[Link]](https://zenodo.org/records/15081506)
| GRAPE | 81 | M,D | 50° | 1444×1444 | CN | G | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| HRF | 45 | M | 45° | 3504×2336 | DE | DR,G | [[Link]](https://www5.cs.fau.de/research/data/fundus-images/)
| INSPIRE | 15 | D | 30° | 1444×1444 | US | POAG | [[Link]](https://eye.medicine.uiowa.edu/inspire-datasets)
| LES-AV | 22 | D | 30° | 1620×1444 | BE | G | [[Link]](https://figshare.com/articles/dataset/LES-AV_dataset/11857698)
| Leuven-Haifa | 240 | D | 30° | 1444×1444 | BE | G | [[Link]](https://rdr.kuleuven.be/dataset.xhtml?persistentId=doi:10.48804/Z7SHGO)
| MAGREBHIA | 69 | M,D | 30° | 1444×1444 | NAf | G |[[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| MESSIDOR-AV | 66 | M | 45° | 1444×1444 | FR | DR | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| PAPILA | 78 | D | 30° | 1444×1444 | ES | G | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)

---

### In-Distribution Test Datasets

| Dataset | N | Center | FOV | Resolution (px) | Region | Pathologies | Download |
|--------|---|--------|-----|----------------|--------|-------------| --------|
| DualModal | 30 | M | 45° | 1024×1024 | CN | H | [[Link]](https://ieee-dataport.org/documents/dualmodal2019-dataset#files) |
| UNAF | 15 | D | 45° | 1444×1444 | PY | DR | [[Link]](https://iopscience.iop.org/article/10.1088/1361-6579/ad3d28) |

---

### Out-of-Distribution Test Datasets

| Dataset | N | Center | FOV | Resolution (px) | Region | Pathologies | Download |
|--------|---|--------|-----|----------------|--------|-------------| --------|
| AV-WIDE | 26 | M | 200° | 829×1531 | US | AMD | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| IOSTAR-AV | 30 | M,D | 45° | 1024×1024 | NL | -- | [[Link]](https://www.retinacheck.org/download-iostar-retinal-vessel-segmentation-dataset)
| MBRSET | 30 | M | 30° | 1444×1444 | BR | DR | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)
| RAVIR | 36 | D | 30° | 768×768 | US | DR,HR |[[Link]](https://ravirdataset.github.io/data/)
| TREND-AV | 48 | M | 45° | 1444×1444 | ME | H | [[Link]](https://www.kaggle.com/datasets/ba1b909c12dbe6c08df00b3ee6fc22d2fef632870359f91384b9001a870f67bf)

---

### Distribution Shift Illustration

![Distribution shifts across datasets](figures/distribution_shift.png)

*Illustrative retinal fundus images highlighting distribution shifts across datasets. **Left:** in-distribution (ID, green) examples from HRF, LES-AV, INSPIRE, and DRIVE used for model development. **Right:** representative out-of-distribution (OoD) images from near-OoD (TREND-AV, IOSTAR-AV, and MBRSET; yellow) and far-OoD (AV-WIDE, RAVIR; red) datasets showcasing substantial variability in imaging conditions, brightness, resolution, and field-of-view.*

## Pre-trained Models

### OCULARNet

Download the OCULARNet pre-trained weights from Hugging Face:

```bash
wget https://huggingface.co/<USERNAME>/OCULARNet/resolve/main/OCULARNet.pth
```

- Model: `base_unet_repvgg_b3`
- Classes: background, artery, vein, crossings

### OCULARNet-nano (5-fold Ensemble)

Download the OCULARNet-nano ensemble weights from Hugging Face:

```bash
wget https://huggingface.co/<USERNAME>/OCULARNet-nano/resolve/main/nano_f1.pth
wget https://huggingface.co/<USERNAME>/OCULARNet-nano/resolve/main/nano_f2.pth
wget https://huggingface.co/<USERNAME>/OCULARNet-nano/resolve/main/nano_f3.pth
wget https://huggingface.co/<USERNAME>/OCULARNet-nano/resolve/main/nano_f4.pth
wget https://huggingface.co/<USERNAME>/OCULARNet-nano/resolve/main/nano_f5.pth
```

- Model: `base_unet_repvgg_a0`
- Classes: background, artery, vein, crossings

## Installation & Dependencies
Clone the repository and install dependencies:

```bash
git clone https://github.com/GonzaloPlaaza/OCULAR.git
cd OCULAR
pip install -r requirements.txt
```

## Inference

- **Images** (RGB format, already cropped) should be placed in:
  `data/images/`

- **Model weights** should be placed in:

  **OCULARNet**
  ```text
  pretrained_weights/OCULARNet/OCULARNet.pth
  ```

  **OCULARNet-nano (5-fold ensemble)**
  ```text
  pretrained_weights/OCULARNet-nano/
  ├── nano_f1.pth
  ├── nano_f2.pth
  ├── nano_f3.pth
  ├── nano_f4.pth
  └── nano_f5.pth
  ```

- **Run inference** with the full OCULARNet model:

  ```bash
  python inference.py \
      --input_dir data/images \
      --output_dir segmentations/ \
      --weights pretrained_weights/OCULARNet/OCULARNet.pth \
      --device cuda
  ```

- **Run inference** with the OCULARNet-nano ensemble:

  ```bash
  python inference.py \
      --input_dir data/images \
      --output_dir segmentations/ \
      --weights pretrained_weights/OCULARNet-nano/nano_f1.pth \
      --ensemble \
      --device cuda
  ```

---

## Zone Extraction

This script processes a `data/` folder containing retinal fundus images and associated vessel/optic disc/crossings segmentations to generate **junction and vessel zone masks** used for analysis. For each image:

- **Bifurcations (arteries & veins):**  
  Extracts geometric bifurcation points using the PBVM method (`handle_interpoints`) and produces a binary ROI mask (20px diameter) per vessel type.

- **Major vasculature (arteries & veins):**  
  Computes major vessel regions either via morphological opening relative to the optic disc size (`mode="opening"`) or by selecting the largest connected components (`mode="largest_cc"`). User can also manually input the footprint width (fw), we recommend values between 2-4 and visual inspection of the results.

- **Crossings ROIs:**  
  Generates circular masks (20px diameter) around the centroids of crossing regions detected in the input segmentation.

### Folder Requirements

The following folders must exist inside the root `data/` directory:

- `images/` — RGB fundus images (`.png`)
- `artery/` — binary artery segmentation masks (`.png`)
- `vein/` — binary vein segmentation masks (`.png`)
- `optic_disc/` — optic disc segmentation masks (`.png`)
- `crossings/` — binary crossings segmentation masks (`.png`)

Each file should be named consistently across all folders.

### Example Data Download

[Download example data.zip](https://drive.google.com/file/d/1x01n3sbI_QUy8DxjQ2KSqKypHZpYw8Wy/view?usp=drive_link)


### How to Run

```bash
python extract_zones.py --data_root /path/to/data \
                        --image_type ODC \
                        --fw 3
```

---

## Citation

If you use OCULAR or OCULARNet in your work, please cite:

```bash
@inproceedings{OCULAR2026,
  title={Beyond Dice: Clinically Meaningful Large-Scale Retinal Artery/Vein Segmentation},
  author={boring guys},
  booktitle={MICCAI 2026},
  year={2026}
}
```
