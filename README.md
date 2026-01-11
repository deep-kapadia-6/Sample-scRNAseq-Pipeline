# sample-scRNAseq 🧬

A comprehensive single-cell RNA sequencing (scRNA-seq) analysis pipeline demonstrating quality control, dimensionality reduction, clustering, cell type annotation, and differential expression analysis using modern Python bioinformatics tools.

## 📋 Overview

This repository contains a complete Jupyter Notebook workflow for analyzing 10X Genomics single-cell RNA sequencing data. The pipeline covers the full analysis workflow from raw count matrices to annotated cell populations with differential gene expression analysis.

**Key Analysis Steps:**
- Loading and preprocessing 10X Genomics data (barcodes, features, matrix)
- Quality control and filtering (mitochondrial content, gene counts)
- Data normalization and variable gene selection
- Dimensionality reduction with scVI (deep learning-based)
- UMAP visualization and Leiden clustering
- Automated cell type annotation using CellTypist
- Differential expression analysis across clusters
- Custom visualization and data export

## ✨ Features

- **Modern scRNA-seq Stack**: Leverages Scanpy, scVI-tools, and CellTypist
- **Deep Learning Integration**: Uses scVI for robust dimensionality reduction and batch correction
- **Automated Cell Typing**: CellTypist integration for rapid cell type annotation
- **Transcription Factor Focus**: Filters data to focus on transcription factor genes
- **Quality Visualizations**: Generates publication-ready plots (UMAP, violin plots, heatmaps)
- **Reproducible Workflow**: Complete end-to-end analysis in a single notebook
- **Model Persistence**: Saves trained scVI models for reproducibility

## 🚀 Installation

### Prerequisites

- Python 3.9+ (tested on Python 3.10)
- Jupyter Notebook or JupyterLab
- ~2GB RAM minimum for the example dataset
- GPU recommended (but not required) for scVI training

### Setup

1. Clone the repository:
bash
git clone https://github.com/deep-kapadia-6/sample-scRNAseq.git
cd sample-scRNAseq

2. Create a conda environment (recommended)
bash
conda create -n scrnaseq python=3.10
conda activate scrnaseq

4. Install required dependencies:
bash
pip install -r requirements.txt

The following packages will be installed:
- anndata - Annotated data structures for single-cell data
- matplotlib - Data visualization
- mudata - Multi-modal data handling
- muon - Multi-omics analysis framework
- scanpy - Single-cell analysis in Python
- scvi - Deep generative models for single-cell omics
- numpy - Numerical computing
- pandas - Data manipulation

## 💻 Usage

### Running the Analysis

1. Launch Jupyter Notebook:
bash
jupyter notebook scRNAseq_code.ipynb

2. Prepare your data files:
Place 10X Genomics output files in the appropriate directory:
- file_barcodes.tsv.gz
- file_features.tsv.gz
- file_matrix.mtx.gz

Update file paths in the notebook to match your data location

3. Execute cells sequentially to run the full analysis pipeline

### Input Data Format

The pipeline expects standard 10X Genomics output:
- Barcodes: Cell barcodes (one per row)
- Features: Gene identifiers (one per row)
- Matrix: Sparse count matrix (Market Exchange Format)

### Customization

Filter Thresholds (Cell 16):
python
sc.pp.filter_cells(rna_adata, min_genes=200)  # Minimum genes per cell
sc.pp.filter_genes(rna_adata, min_cells=3)    # Minimum cells per gene
rna_adata = rna_adata[rna_adata.obs.pct_counts_mt < 40, :]  # Mitochondrial content

scVI Model Parameters:
python
model = scvi.model.SCVI(adata)  # Default: n_latent=10, n_hidden=128
model.train()  # Default: 400 epochs

Clustering Resolution:
python
sc.tl.leiden(adata, resolution=0.5, key_added="leiden_scVI")  # Adjust resolution

## 📂 Project Structure

sample-scRNAseq/
├── scRNAseq_code.ipynb      # Main analysis notebook
├── requirements.txt          # Python dependencies
├── README.md                # This file
├── LICENSE                  # MIT License
└── .gitignore              # Git ignore file

## 🔬 Analysis Workflow

1. Data Loading & QC
- Load 10X data into AnnData object
- Calculate QC metrics (genes per cell, UMI counts, mitochondrial %)
- Visualize distributions before filtering

2. Preprocessing
- Filter low-quality cells and genes
- Normalize counts (CPM normalization)
- Log-transform data
- Filter for transcription factor genes (optional)

3. Dimensionality Reduction (scVI)
- Train variational autoencoder on count data
- Generate 10-dimensional latent representation
- Save trained model for reproducibility

4. Clustering & Visualization
- Compute k-nearest neighbors graph
- Leiden clustering algorithm
- UMAP for 2D visualization
- Force-directed graph layout (ForceAtlas2)

5. Cell Type Annotation
- Automated annotation using pre-trained CellTypist models
- Majority voting for cluster-level annotation
- Confidence scoring for predictions

6. Differential Expression
- Identify marker genes for each cluster
- t-test or logistic regression methods
- Rank genes by statistical significance

7. Visualization & Export
Generate publication-quality plots

Export annotated AnnData object (.h5ad format)

## 📊 Example Output

The pipeline generates several key visualizations:
- QC Plots: Violin plots showing genes/cell, UMI counts, and mitochondrial percentage
- UMAP Plots: Colored by cluster ID, cell type annotation, or gene expression
- Dotplots: Cluster composition and annotation confidence
- Heatmaps: Top marker genes per cluster
- Pie Charts: Cell type proportions

## 🛠️ Technical Details

Computational Requirements
- Memory: ~2-4 GB for small datasets (<5k cells), scales with data size
- Runtime: ~5-15 minutes for full pipeline (depends on dataset size and CPU/GPU)
- GPU: Optional but recommended for faster scVI training

Key Technologies
- Scanpy: Industry-standard scRNA-seq analysis framework
- scVI-tools: State-of-the-art probabilistic models for single-cell genomics
- CellTypist: Automated cell type annotation using machine learning
- AnnData: Efficient storage format for annotated data matrices
- PyTorch: Deep learning backend for scVI

Data Specifications
- Cells: Designed for 1k-10k cells (scalable to larger datasets)
- Genes: Full transcriptome or subset (TF-focused in example)
- Format: Sparse matrix (memory-efficient for scRNA-seq data)

## 📝 Citation

If you use this pipeline, please cite the underlying tools:

- Scanpy: Wolf et al. (2018) Genome Biology
- scVI: Lopez et al. (2018) Nature Methods
- CellTypist: Domínguez Conde et al. (2022) Science
- AnnData: Virshup et al. (2021) bioRxiv

## 🤝 Contributing
Contributions are welcome! Areas for improvement:

- Add support for multi-sample integration
- Implement trajectory inference analysis
- Add RNA velocity analysis
- Create command-line interface
- Add automated report generation
- Improve documentation with example datasets
- Please open an issue or submit a pull request.

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments
- Analysis pipeline adapted from best practices in single-cell genomics
- Developed for research in leukemia immunobiology and stem cell biology
- Built using open-source bioinformatics tools from the Python ecosystem

## 📧 Contact
Created by @deep-kapadia-6

For questions about the analysis or to report issues, please open a GitHub issue.

Note: This pipeline is for research purposes only. Ensure you have appropriate permissions and ethical approvals before analyzing human genomic data.
