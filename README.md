# Enhanced Image Processing for Foundation Model Cell Segmentation: A Study on Challenging Modalities

## Overview
This repository contains the code, data logs, and visualizations for the study **"Enhanced Image Processing for Foundation Model Cell Segmentation: A Study on Challenging Modalities"**. 

Recent foundation models achieve state-of-the-art performance in cell segmentation but struggle with challenging imaging modalities such as H&E-stained histology and multiplex immunofluorescence (mIF). This project systematically evaluates whether advanced classical image processing techniques (CLAHE, Laplacian sharpening, Color Deconvolution) can rescue the zero-shot segmentation performance of four distinct architectures (**SAM, DeepCell, HoVer-Net, and Cellpose**) without the computational burden of fine-tuning.

## Repository Structure

The repository is organized to ensure full transparency and reproducibility of the experimental results:

* **`notebooks/`**
    * `Model_Inference.ipynb`: The main execution pipeline evaluating baseline and optimized model performances.
    * `Hyperparameter_Tuning.ipynb`: The grid search execution for establishing optimal preprocessing parameters.
* **`results/`**
    * `*.csv`: The final, optimized quantitative results (Dice, Panoptic Quality, etc.) for each model using their ideal preprocessing pipelines.
    * `experiment_baseline/`: The quantitative results for models run "as-is" on raw H&E and mIF imagery with no preprocessing.
    * `hyperparameter_tuning/`: Raw grid search data tracking performance dynamics across different parameter values (e.g., clip limits, kernel sizes).
    * `figures/`: All generated visual artifacts, including comparative bar charts and tuning dynamic plots featured in the paper.

## Key Findings
* **Domain Rescue via Color Deconvolution:** Isolating the hematoxylin channel is strictly mandatory for rescuing fluorescence-trained models like DeepCell on H&E tissue, elevating zero-shot Dice scores by over +0.340.
* **The CLAHE Paradox:** Simple contrast enhancement (CLAHE) actively degraded DeepCell's performance on H&E by violating its inductive bias and amplifying background noise.
* **mIF Edge Enhancement:** Laplacian sharpening provided consistent boundary delineation improvements for generalist transformers like SAM but risked over-segmentation in robust specialists like Cellpose.

*(See `results/figures/HE_Dice_BarChart.png` and `results/figures/mIF_Dice_BarChart.png` for a visual breakdown of performance)*

## Replication Instructions
All experiments were conducted in a highly controlled Python environment utilizing PyTorch, OpenCV, and scikit-image.

1.  Open the notebooks in Google Colab.
2.  Ensure the runtime is set to utilize an **NVIDIA T4 GPU** (or equivalent) to manage VRAM limits during standard inference.
3.  Execute the cells sequentially. The notebooks are configured to automatically fetch the necessary subsets of the **CellBinDB** dataset, perform the image patching/blending logistics, and output the quantitative CSVs.
