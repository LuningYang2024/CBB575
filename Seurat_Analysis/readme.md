# Single-cell RNA-seq Analysis Tutorial

## Overview
This tutorial demonstrates how to analyze single-cell RNA-seq data from the GEO database using Seurat. The workflow includes data preprocessing, integration with patient metadata and cell type annotations, dimensionality reduction, and visualization using UMAP. Due to the large size of the dataset, it is hosted on Google Drive for access.

## Data
The dataset for this project is available from GEO under accession number **GSE131907**. It contains single-cell RNA-seq data from lung cancer samples. The preprocessed data is hosted on Google Drive for convenience:

[Download GSE131907 Preprocessed Data]()

## Steps Performed in the Analysis

### 1. Download and Import Data
- Data was downloaded from GEO and stored in `.rds` and `.csv` formats.
- The raw UMI matrix (`GSE131907_Lung_Cancer_raw_UMI_matrix.rds`) and metadata files were used as inputs for creating a Seurat object.

### 2. Add Metadata and Cell Type Annotation
- Patient metadata and cell type annotations were incorporated into the Seurat object to enable sample-specific and cell-type-specific analysis.
- Metadata files include:
  - `GSE131907_Lung_Cancer_Feature_Summary.csv`
  - `GSE131907_scrna_anno.csv`
- Cell type annotations were aligned with the barcodes to annotate individual cells with detailed classifications.

### 3. Save Intermediate Seurat Objects
- Intermediate Seurat objects were saved at various stages to facilitate downstream analysis without reprocessing.
- Examples of saved objects:
  - `GSE131907_filtered_lung_seurat.rds`: Annotated and filtered object.
  - `GSE131907_seurat_preprocessed_meta_paired.rds`: Preprocessed Seurat object with paired samples.

### 4. Data Preprocessing
- Normalization and variable feature selection were performed using the Seurat pipeline.
- Dimensionality reduction was achieved through PCA.
- Harmony integration was used to correct batch effects based on sample metadata.

### 5. Dimensionality Reduction and Visualization
- UMAP was performed to reduce data dimensionality for visualization.
- Clustering and exploratory analysis were conducted to visualize metadata attributes such as:
  - Sample
  - Patient ID
  - Tissue Origins
  - Disease Label
  - Cell Type and Subtype
  - Smoking Status
  - Cancer Stages

### 6. UMAP Plots
- Plots were generated to explore the relationships between different metadata categories and the clusters derived from UMAP.

## File Structure
- **R Markdown File:** `seurat_analysis_tutorial.Rmd`
- **Intermediate Data Files:**
  - `GSE131907_filtered_lung_seurat.rds`
  - `GSE131907_seurat_preprocessed_meta_paired.rds`
- **Metadata Files:**
  - `GSE131907_Lung_Cancer_Feature_Summary.csv`
  - `GSE131907_scrna_anno.csv`

## Requirements
To reproduce this analysis, ensure the following dependencies are installed:

- R packages:
  - `Seurat`
  - `dplyr`
  - `readxl`
  - `ggplot2`
  - `harmony`
  - `patchwork`

## How to Run the Analysis
1. Clone the repository and download the dataset from the provided Google Drive link.
2. Install required R packages.
3. Run the R Markdown file `seurat_analysis_tutorial.Rmd` in RStudio.

```bash
# Example Command to Install Required R Packages
install.packages(c("Seurat", "dplyr", "ggplot2", "harmony", "patchwork"))
```

4. Follow the steps in the R Markdown file for detailed explanations and code.

## Notes
- The dataset is too large to be hosted on GitHub. Please download it from Google Drive.
- Intermediate Seurat objects can be used to skip certain preprocessing steps.
- Ensure sufficient memory is available when running the analysis.

## Contact
For any questions or suggestions, feel free to reach out:

**Author:** Luning Yang  
**Email:** luning.yang@yale.edu

