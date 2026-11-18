# Sugarcane cultivar identification under controlled conditions: a deep learning classification approach

Code and notebooks for the manuscript *Sugarcane cultivar identification under controlled conditions: a deep learning classification approach* (J. C. de Lemos and A. F. de Angelis), submitted to PLOS ONE.

- Image dataset (REDU, Version 2): <https://doi.org/10.25824/redu/S1OJFI>
- Source code deposit (REDU, Version 1): <https://doi.org/10.25824/redu/1Q9RHV>

This GitHub repository mirrors the code deposit in REDU. If the two differ, the REDU deposit is the reference version.

## What the study does

A YOLOv8n-cls model (Ultralytics) was fine-tuned end to end to classify four sugarcane cultivars (CTC-1007, IAC-5503, RB-5375, RB-7515) from photographs of individual stalk sections (billets) taken under controlled lighting on a black background. The dataset has 1,476 cropped images.

Near-duplicate images (same photograph, or photographs differing only marginally) were found in the dataset. All reported results therefore use a grouped partition: near-duplicates are always assigned to the same subset (train, validation, or test). The 10 repeated runs use 1,031 / 220 / 225 images for training, validation, and test.

## Main results (grouped partition, 10 runs, n = 225 test images per run)

| Quantity | Value |
|---|---|
| Mean Top-1 accuracy (± SD) | 0.9818 ± 0.0121 |
| 95% confidence interval of the mean (t, 10 runs) | [0.9731, 0.9904] |
| Range of per-run accuracy | 0.9556 to 0.9956 |
| Reference run (seed 101) | 223/225 = 0.9911 (95% Wilson interval [0.9682, 0.9976]) |
| Color-histogram SVM, same 224 x 224 crop | 0.9164 ± 0.0273 |

The 10 runs repartition the same 1,476 images, so they are not independent samples of the cultivar population. The intervals describe variability due to random partitioning and initialization, not uncertainty about new billets or culms. Images of the same physical billet in different positions are not identified as near-duplicates and may appear in different subsets. See the Limitations section of the paper.

## Repository contents

| File | Purpose |
|---|---|
| `preprocessing.ipynb` | Two-stage OpenCV pipeline: thresholding and contour detection, rectangular bounding-box cropping, and polygon-based segmentation. |
| `looking_for_duplicates.ipynb` | Finds near-duplicate image pairs and builds the groups used by the grouped partition (outputs `dimensoes_grupos.csv` and `duplicatas.csv`). |
| `Rodadas_agrupadas_PLOS_ONE.ipynb` | Grouped partition, training of the 10 runs, and color-histogram baselines. |
| `Partes_D_E_figuras_e_metricas.ipynb` | Test predictions, confusion matrix, class-wise metrics, learning curve, Grad-CAM, and inference latency. |
| `S1_reference_run_grouped_YOLOv8n_cls.ipynb` | A single complete run (preprocessed images to test metrics) with the grouped partition. |

Names of some output files and CSV columns are in Portuguese (for example `classe`, `arquivo`, `grupo`) to match the files already generated and cited in the paper.

## Software environment used for the reported results

Python 3.13, Ultralytics 8.4.208, PyTorch 2.14.1 (CUDA 13.0), NumPy 2.4.6, Pandas 2.2.3, SciPy 1.16.3. Training and latency measurements were run on an NVIDIA Tesla T4 GPU (Google Colab). Training is not bit-for-bit reproducible on GPU even with the Ultralytics default `deterministic = True`: a re-execution of the reference run (S1) gave 224/225 = 0.9956 instead of 223/225 = 0.9911, within the range of the 10 runs.

## How to run

1. Download the image dataset from the REDU DOI above and unzip it. Run `preprocessing.ipynb` to obtain the cropped images (`data_cropped`, one sub-folder per cultivar).
2. Run `looking_for_duplicates.ipynb` to build the near-duplicate groups.
3. Run `Rodadas_agrupadas_PLOS_ONE.ipynb` and then `Partes_D_E_figuras_e_metricas.ipynb`, or run `S1_reference_run_grouped_YOLOv8n_cls.ipynb` alone for a single run.
4. In each notebook, set the path variables in the first configuration cell to the folders on your Google Drive or local disk.

## Citation

If you use this code or the dataset, please cite the paper and the REDU records:

- Image dataset: Angelis, A. F. de. *High-quality image dataset of stalks from four sugarcane cultivars captured under controlled conditions*. Repositório de Dados de Pesquisa da Unicamp, V2, 2025. <https://doi.org/10.25824/redu/S1OJFI>
- Source code: Lemos, J. C. de; Angelis, A. F. de. *Source code for: Sugarcane cultivar identification under controlled conditions: a deep learning classification approach*. Repositório de Dados de Pesquisa da Unicamp, V1, 2026. <https://doi.org/10.25824/redu/1Q9RHV>

## License

See the license stated in the REDU deposit records.
