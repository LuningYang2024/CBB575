# **CBB575: LUAD Bioinformatics Analysis Project**  

## **Objective**  
The goal of this project is to investigate the tumor microenvironment and gene expression patterns in lung adenocarcinoma (LUAD). Specifically, we:
1. Explore single-cell RNA sequencing data for exploratory analysis.  
2. Perform bulk RNA-seq analysis to detect differentially expressed genes (DEGs).  
3. Examine tumor and patient-specific gene signatures to study heterogeneity.  

---

## **Project Structure**  
The repository contains three main analysis components:  

### 1. **Seurat_Analysis**  
- **Objective**: Analyze single-cell RNA sequencing data using the Seurat package in R.  
- **Steps**:  
   - Preprocess single-cell RNA-seq data (filtering, normalization, scaling).  
   - Perform clustering and UMAP visualization to identify patterns based on disease status, smoking history, and cell types.  
- **Expected Results**:  
   - UMAP plots showing transcriptional clusters.  

### 2. **Downstream_Bulk_RNA**  
- **Objective**: Perform DEG analysis and pathway enrichment using bulk RNA-seq data.  
- **Steps**:  
   - Identify DEGs between paired tumor and control samples.  
   - Use hierarchical clustering to group genes by expression profiles.  
   - Conduct pathway enrichment analysis with Cytoscape (DAVID/gProfiler).  
- **Expected Results**:  
   - Heatmaps showing gene expression variation (tumor vs. control).  
   - Enrichment maps highlighting key biological pathways.  

### 3. **Tumor and Patient Signatures/**  
- **Objective**: Identify tumor- and patient-specific gene signatures to explore heterogeneity.  
- **Steps**:  
   - Preprocess data: filter and normalize expression values, replace missing values.  
   - Perform hierarchical clustering to identify expression-based clusters.  
   - Use silhouette scores to determine optimal clustering thresholds.  
   - Conduct statistical analysis to find significantly differentially expressed genes between clusters.  
   - Analyze patient metadata for differences (e.g., smoking, stage).  
- **Expected Results**:  
   - Clusters of patient- and tumor-specific gene signatures.  
   - Pathway annotations for potential therapeutic targets.  

---

## **Dependencies**  
Install the following tools and libraries before running the analyses:  
- **R**: Seurat, ggplot2, dplyr  
- **Python**: pandas, scipy, sklearn, matplotlib  
- **Cytoscape**: For pathway enrichment maps  

---

## **How to Run**  
1. Clone the repository:  
   ```bash
   git clone https://github.com/your_username/luad_bioinformatics.git
   cd luad_bioinformatics
   ```  

2. Follow the step-by-step analysis in each folder:  
   - **Seurat_Analysis**: Run the R scripts for single-cell analysis.  
   - **Downstream_Bulk_RNA**: Execute the bulk RNA analysis pipeline.  
   - **Tumor and Patient Signatures**: Follow the Python notebook for tumor/patient clustering and enrichment.  

---

## **Results**  
- UMAP visualizations for transcriptional heterogeneity.  
- Heatmaps showing gene expression variations.  
- Pathway enrichment maps for functional analysis.  

---
