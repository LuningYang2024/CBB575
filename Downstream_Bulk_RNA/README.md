# Gene Expression Analysis Workflow

A comprehensive workflow for analyzing gene expression data, performing PCA (Principal Component Analysis), identifying differentially expressed genes, and visualizing results through PCA plots and heatmaps. This pipeline is optimized for comparing tumor vs. normal lung tissue gene expression data, or other metadata-derived subgroups.

## Overview

This analysis pipeline processes CSV files containing gene expression data with associated metadata for each sample (e.g., tissue origin, histology, smoking status). Key features include:

- Data loading and validation
- PCA for dimensionality reduction and sample clustering visualization
- Statistical analysis for differential expression
- Gene ranking and filtering based on customizable criteria
- Clustered heatmap visualization of results


## Requirements

### Dependencies

Create a virtual environment and install the required packages:

```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### requirements.txt

```
pandas>=1.3.0
numpy>=1.20.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=0.24.0
scipy>=1.7.0
jupyter>=1.0.0
```

## How to Run the Analysis

1. **Setup**
   ```bash
   # Clone the repository
   git clone https://github.com/LuningYang2024/CBB575.git
   cd CBB575/Downstream_Bulk_RNA

   # Create and activate virtual environment
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate

   # Install dependencies
   pip install -r requirements.txt
   ```

2. **Data Preparation**
   ```bash
   # Place your expression data in the input directory
   # Make sure your data follows the required format:
   # - Contains necessary metadata columns
   # - Gene expression values are normalized
   # - Saved as a CSV file
   ```

3. **Run Analysis**
   Using Jupyter Notebook
   ```bash
   # Start Jupyter notebook server
   jupyter notebook

   # Open and run the main analysis notebook:
   Analysis_Downstream.ipynb
   ```

## Data Requirements

The data contains:
- One row per sample
- Metadata columns (`Patient_id`, `Sample`, `Tissue_origin`, `Histology`, `Sex`, `Age`, `Smoking`, `Pathology`, `EGFR`, `Stage`)
- Gene expression columns (one per gene)
- Log2 transformed expression values (or transformable)

## Dependencies

- Python 3.7+
- Required packages:
  ```bash
  pip install pandas numpy matplotlib seaborn scikit-learn scipy
  ```
## Usage Guide / Analysis Workflow

### 1. Data Loading and Examination
This step involves loading and initial exploration of the RNA-seq dataset.

```python
import pandas as pd

# Load the dataset
data = pd.read_csv('combined_expression_data.csv')

# Examine dataset structure
print("Dataset Shape:", data.shape)
print("\nColumns Overview:")
print(data.info())

# Check for missing values
print("\nMissing Values:")
print(data.isnull().sum())
```

**Key Components:**
- Dataset contains 20 samples with >29,000 features
- Includes metadata (`Patient_id`, `Tissue_origin`, etc.) and gene expression values
- Initial data quality assessment and missing value detection
- Understanding data structure before analysis

### 2. Data Preprocessing and PCA Analysis
This stage prepares the data for analysis and performs dimensionality reduction.

```python
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
import seaborn as sns

# Separate metadata and expression data
metadata_cols = ['Patient_id', 'Tissue_origin', 'Histology', 'Smoking']
expression_data = data.drop(metadata_cols, axis=1)

# Standardize expression data
scaler = StandardScaler()
scaled_expression = scaler.fit_transform(expression_data)

# Perform PCA
pca = PCA(n_components=2)
pca_result = pca.fit_transform(scaled_expression)

# Create visualization
plt.figure(figsize=(10, 8))
scatter = plt.scatter(pca_result[:, 0], pca_result[:, 1], 
                     c=data['Tissue_origin'].map({'nLung': 'green', 'tLung': 'red'}))
plt.xlabel(f'PC1 ({pca.explained_variance_ratio_[0]:.1%} variance)')
plt.ylabel(f'PC2 ({pca.explained_variance_ratio_[1]:.1%} variance)')
```

**Key Steps:**
- Isolation of numeric gene expression data
- Standardization to mean=0 and unit variance
- PCA computation for dimensionality reduction
- Visualization of sample clustering patterns

### 3. Differential Expression Analysis
Identifies genes with significant expression differences between groups.

```python
from scipy import stats
import numpy as np

def analyze_differential_expression(data, group_col='Tissue_origin'):
    # Perform statistical tests
    results = []
    for gene in expression_cols:
        group1 = data[data[group_col] == 'nLung'][gene]
        group2 = data[data[group_col] == 'tLung'][gene]
        
        # Calculate statistics
        t_stat, p_val = stats.ttest_ind(group1, group2, equal_var=False)
        fold_change = np.log2(group2.mean() / group1.mean())
        cohens_d = (group2.mean() - group1.mean()) / np.sqrt((group2.var() + group1.var()) / 2)
        
        results.append({
            'gene': gene,
            'p_value': p_val,
            'fold_change': fold_change,
            'effect_size': cohens_d
        })
    
    return pd.DataFrame(results)

# Run analysis
diff_expr_results = analyze_differential_expression(data)
significant_genes = diff_expr_results[
    (diff_expr_results['p_value'] < 0.05) & 
    (abs(diff_expr_results['fold_change']) > 1.0)
]
```

**Analysis Components:**
- Welch's t-test for group comparisons
- Fold change calculation (log2 scale)
- Effect size quantification (Cohen's d)
- Significance thresholds: p < 0.05, |fold change| > 1.0

### 4. Heatmap Visualization
Creates a clustered visualization of expression patterns.

```python
# Generate heatmap for top differential genes
def create_expression_heatmap(data, genes, metadata_col):
    plt.figure(figsize=(12, 8))
    g = sns.clustermap(
        data[genes], 
        col_cluster=True,
        row_cluster=True,
        cmap='RdBu_r',
        z_score=0,
        col_colors=data[metadata_col].map({'nLung': 'green', 'tLung': 'red'}),
        figsize=(15, 10)
    )
    return g

# Select top genes and create heatmap
top_genes = significant_genes.nsmallest(50, 'p_value')['gene']
g = create_expression_heatmap(data, top_genes, 'Tissue_origin')
plt.savefig('results/figures/expression_heatmap.png', dpi=300, bbox_inches='tight')
```

**Visualization Features:**
- Hierarchical clustering of genes and samples
- Z-score normalization for comparability
- Metadata annotations for sample groups
- Red-Blue color scheme for expression levels

### 5. Results Output
Final analysis products and data organization:

```python
# Save analysis results
results['Feature_intrested'].to_csv('differential_genes.csv')
```


## Example Results

- **PCA Visualization**: Clear separation between normal and tumor samples
- **Gene Expression**: Identification of significant markers (e.g., FABP4, IGLC2)
- **Heatmaps**: Hierarchical clustering of gene expression patterns

## Important Notes

1. Ensure expression data is properly normalized/log-transformed
2. Handle missing data appropriately
3. Consider multiple testing corrections for large gene sets
4. Adjust statistical thresholds based on your specific needs

## Citation

If you use this workflow in your research, please cite:

```bibtex
@software{gene_expression_workflow,
  author = {Ovali, Yusuf Sina},
  title = {Downstream Analysis Pipeline for Bulk RNA-Seq Data},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  url = {https://github.com/LuningYang2024/CBB575/tree/main/Downstream_Bulk_RNA}
}
```
