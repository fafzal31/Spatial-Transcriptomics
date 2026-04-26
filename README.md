<div align="center">

# 🧬 Spatial Transcriptomics Analysis Suite

### A comprehensive exploration of single-cell spatial omics using Scanpy & Squidpy

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)](https://python.org)
[![Scanpy](https://img.shields.io/badge/Scanpy-1.10%2B-orange?style=for-the-badge)](https://scanpy.readthedocs.io)
[![Squidpy](https://img.shields.io/badge/Squidpy-1.4%2B-green?style=for-the-badge)](https://squidpy.readthedocs.io)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-F37626?style=for-the-badge&logo=jupyter)](https://jupyter.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

<br/>

> *Decoding the spatial language of cells — where gene expression meets tissue architecture.*

<br/>

<img src="https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_14_0.png" width="80%" alt="Spatial scatter overview"/>

</div>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Repository Structure](#-repository-structure)
- [Notebook 1 — Scanpy Spatial Basics (Visium + MERFISH)](#-notebook-1--scanpy-spatial-basics-visium--merfish)
- [Notebook 2 — Visium H&E Analysis with Squidpy](#-notebook-2--visium-he-analysis-with-squidpy)
- [Notebook 3 — Visium Fluorescence Analysis with Squidpy](#-notebook-3--visium-fluorescence-analysis-with-squidpy)
- [Notebook 4 — Xenium In Situ Analysis with Squidpy](#-notebook-4--xenium-in-situ-analysis-with-squidpy)
- [Key Concepts](#-key-concepts)
- [Installation](#-installation)
- [Results Gallery](#-results-gallery)
- [References & Tutorials](#-references--tutorials)

---

## 🔭 Overview

This repository contains four end-to-end spatial transcriptomics analysis notebooks that span the most important spatial omics platforms in use today — **10x Visium**, **MERFISH**, and **10x Xenium**. Each notebook follows an official tutorial from the Scanpy / Squidpy ecosystem and extends it with a complete, reproducible workflow covering:

| Step | Description |
|---|---|
| 📥 **Data loading** | Platform-specific loaders (AnnData, SpatialData, Zarr) |
| 🔬 **Quality control** | Per-cell metrics, mitochondrial content, control probes |
| ⚙️ **Preprocessing** | Normalization, log-transform, HVG selection |
| 📊 **Dimensionality reduction** | PCA → k-NN graph → UMAP |
| 🔵 **Clustering** | Leiden community detection |
| 🗺️ **Spatial statistics** | Neighborhood enrichment, co-occurrence, Moran's I |
| 🧪 **Image analysis** | Morphology features, watershed segmentation |
| 💬 **Cell-cell communication** | Ligand–receptor interaction screening |

---

## 📂 Repository Structure

```
📦 spatial-transcriptomics-suite/
├── 📓 Analysis_and_visualization_of_spatial_transcriptomics_data.ipynb
├── 📓 Analyze_Visium_H_E_data.ipynb
├── 📓 Analyze_Visium_fluorescence_data.ipynb
├── 📓 Analyze_Xenium_Data.ipynb
└── 📄 README.md
```

---

## 📓 Notebook 1 — Scanpy Spatial Basics (Visium + MERFISH)

> **Tutorial:** [Scanpy — Analysis and visualization of spatial transcriptomics data](https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html)

### 🎯 Objective

Introduces the foundational Scanpy workflow for spatial transcriptomics using a **10x Visium Human Lymph Node** dataset, then extends the same framework to a **MERFISH** dataset, demonstrating that the core scanpy API is platform-agnostic.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Visium |
| **Tissue** | Human Lymph Node |
| **Sample ID** | `V1_Human_Lymph_Node` |
| **Extended** | MERFISH (hypothalamus) |

### 🔬 Workflow

```
Load Visium SGE dataset
        │
        ▼
QC Metrics (total_counts, n_genes, % MT)
        │
        ▼
Filter cells & genes
        │
        ▼
Normalize → log1p → HVG selection (top 2000)
        │
        ▼
PCA → Neighbors → UMAP → Leiden clustering
        │
        ▼
Spatial visualization (sc.pl.spatial)
        │
        ▼
Marker gene discovery (t-test rank_genes_groups)
        │
        ▼
─── MERFISH branch ───
Load count matrix + coordinate table
→ Build AnnData with obsm["spatial"]
→ Normalize → Cluster → Spatial embedding
```

### 🖼️ Key Outputs

**1. Quality Control Histograms**

> Distributions of total UMI counts and unique gene counts per spot — used to set filtering thresholds. A bimodal distribution in total counts reveals tissue heterogeneity even before clustering.

![QC histograms](https://scanpy-tutorials.readthedocs.io/en/latest/_images/basic-analysis_7_0.png)

---

**2. UMAP colored by cluster, total counts, and gene counts**

> The UMAP embedding places spots in 2D by transcriptional similarity. Spots colored by cluster reveal distinct transcriptional programs; coloring by QC metrics confirms there is no batch/quality confound driving the separation.

![UMAP](https://scanpy-tutorials.readthedocs.io/en/latest/_images/basic-analysis_14_1.png)

---

**3. Spatial scatter — clusters overlaid on H&E tissue image**

> Leiden clusters projected back onto the tissue section. Spatially contiguous regions sharing a cluster identity correspond to distinct anatomical structures (e.g., germinal centers, mantle zones, T-cell areas in the lymph node).

![Spatial clusters](https://scanpy-tutorials.readthedocs.io/en/latest/_images/basic-analysis_17_0.png)

---

**4. Cropped region + top marker genes**

> Fine-grained spatial inspection of clusters 5 and 9, alongside a heatmap of the top 10 marker genes for cluster 9 — genes such as **CR2** (complement receptor 2, a B-cell marker) confirm correct biological identity of the cluster.

![Heatmap + crop](https://scanpy-tutorials.readthedocs.io/en/latest/_images/basic-analysis_22_0.png)

---

**5. MERFISH spatial embedding**

> The MERFISH dataset (continuous tissue coordinates in microns) is processed with the same pipeline. The spatial embedding confirms that the MERFISH clusters recapitulate known hypothalamic cell-type geography.

![MERFISH](https://scanpy-tutorials.readthedocs.io/en/latest/_images/basic-analysis_33_0.png)

### 💡 Key Takeaways

- `sc.pl.spatial()` overlays any `.obs` column directly on the histology image
- The scanpy preprocessing recipe works out-of-the-box for Visium **and** MERFISH
- Cropping with `crop_coord` enables sub-region inspection without re-clustering

---

## 📓 Notebook 2 — Visium H&E Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Visium H&E data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html)

### 🎯 Objective

Goes beyond simple visualization to perform **spatial statistics** on a Visium mouse brain section stained with H&E. This notebook demonstrates how morphological information extracted from the histology image can complement gene-expression-based clustering.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Visium |
| **Tissue** | Mouse Brain (coronal) |
| **Stain** | Hematoxylin & Eosin (H&E) |
| **Loader** | `sq.datasets.visium_hne_adata()` |

### 🔬 Workflow

```
Load pre-processed Visium H&E AnnData + ImageContainer
        │
        ▼
Spatial scatter — gene-expression clusters on tissue
        │
        ▼
Image Feature Extraction (scale 1.0 × 2.0)
  └── Summary statistics per spot (mean, std, quantiles)
        │
        ▼
Feature-based Leiden clustering
  └── Compare morphology clusters vs. gene-expression clusters
        │
        ▼
Spatial Neighborhood Graph (sq.gr.spatial_neighbors)
        │
        ▼
Neighborhood Enrichment  ──►  Which clusters co-localize?
Co-occurrence Probability ──►  Spatial range of interaction
Ligand-Receptor Screening ──►  Cell-cell communication
Moran's I Autocorrelation ──►  Spatially variable genes
```

### 🖼️ Key Outputs

**1. Gene-expression clusters on H&E image**

> Squidpy's `sq.pl.spatial_scatter` renders cluster labels directly on the H&E tile. Hippocampal subfields (CA1, CA3, dentate gyrus) and cortical layers are cleanly resolved.

![H&E scatter](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_2_0.png)

---

**2. Morphology-based vs. gene-expression-based clusters**

> Image summary features (computed at two scales to capture both local texture and broader tissue context) are clustered independently of gene expression. The strong agreement between the two clustering approaches validates that tissue morphology and transcriptomics carry concordant biological signal.

![Feature clusters](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_11_0.png)

---

**3. Neighborhood Enrichment Heatmap**

> A z-score matrix revealing which cluster pairs are significantly enriched (red) or depleted (blue) in adjacency. The Hippocampus cluster shows strong enrichment with Pyramidal Layer clusters, consistent with known anatomy.

![Neighborhood enrichment](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_14_0.png)

---

**4. Co-occurrence probability**

> For a given source cluster (Hippocampus), the probability of observing each target cluster increases monotonically with spatial radius — capturing short- vs. long-range spatial dependencies.

![Co-occurrence](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_16_0.png)

---

**5. Ligand–Receptor interaction dotplot**

> The `sq.gr.ligrec` permutation test identifies statistically significant L-R pairs between source (Hippocampus) and target clusters. Dot size encodes mean expression; color encodes significance.

![LR interactions](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_18_0.png)

---

**6. Spatially variable genes (Moran's I)**

> Moran's I ranks genes by spatial autocorrelation. Top genes — **Olfm1**, **Plp1**, **Itpka** — display strongly spatially patterned expression, co-localizing with specific anatomical regions.

![Moran genes](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_hne_20_0.png)

### 💡 Key Takeaways

- **Image features at multiple scales** are essential: scale 1.0 captures spot-level texture, scale 2.0 incorporates tissue-level context
- Neighborhood enrichment is a powerful tool to quantify anatomical co-localization without requiring prior annotation
- Moran's I is the gold standard for discovering spatially variable genes

---

## 📓 Notebook 3 — Visium Fluorescence Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Visium fluorescence data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html)

### 🎯 Objective

Demonstrates **cell segmentation from multi-channel fluorescence images** and extraction of per-spot morphological features, showcasing how immunofluorescence (IF) data adds a protein-level layer to Visium gene-expression analysis.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Visium |
| **Imaging modality** | Multi-channel fluorescence |
| **Loader** | `sq.datasets.visium_fluo_adata_crop()` |
| **Channels** | DAPI (nuclei) + tissue marker |

### 🔬 Workflow

```
Load cropped Visium fluorescence dataset + ImageContainer
        │
        ▼
Visualize multichannel image (channelwise=True)
        │
        ▼
Image Preprocessing
  └── Gaussian smoothing (sq.im.process)
        │
        ▼
Cell Segmentation
  └── Watershed algorithm on DAPI channel (sq.im.segment)
        │
        ▼
Segmentation Feature Extraction
  └── Cell count per spot, mean channel intensities
        │
        ▼
Multi-feature extraction pipeline:
  ├── features_orig  (summary + texture + histogram, scale=1.0, mask_circle=True)
  ├── features_context (summary + histogram, scale=1.0)
  └── features_lowres (summary + histogram, scale=0.25)
        │
        ▼
Leiden clustering per feature set
  └── Compare summary / histogram / texture clusters vs. gene-expression clusters
```

### 🖼️ Key Outputs

**1. Raw fluorescence channels (DAPI + marker)**

> The `img.show(channelwise=True)` call reveals the two-channel IF image. The DAPI channel (blue) delineates nuclei; the marker channel highlights specific cell populations.

![Fluorescence channels](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_fluo_4_0.png)

---

**2. Watershed segmentation result**

> After Gaussian smoothing, the watershed algorithm segments individual cells. The right panel shows the segmentation mask overlaid on a 500×500 µm crop — each color represents a distinct cell instance.

![Segmentation](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_fluo_6_0.png)

---

**3. Segmentation-derived feature maps**

> Per-spot cell count (`segmentation_label`) and mean channel intensities are computed from the segmentation mask and projected onto the tissue. These features provide a proxy for local cell density and protein abundance.

![Segmentation features](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_fluo_8_0.png)

---

**4. Multi-resolution feature clustering comparison**

> Three independent Leiden clusterings derived from different feature sets (summary, histogram, texture) are compared to the gene-expression clustering. High concordance across modalities confirms biological robustness; discordant spots highlight tissue regions where morphology and transcriptomics diverge.

![Feature clustering](https://squidpy.readthedocs.io/en/stable/_images/tutorial_visium_fluo_14_0.png)

### 💡 Key Takeaways

- `mask_circle=True` restricts feature extraction to the circular Visium spot footprint — critical for accuracy
- The watershed segmentation is sensitive to the smoothing kernel; tuning `sigma` impacts cell boundary detection
- **Multi-scale feature extraction** (scale 0.25 → 1.0) is the most informative configuration for Leiden clustering from image features
- Segmentation features bridge transcriptomics and proteomics — a first step toward spatial multi-omics

---

## 📓 Notebook 4 — Xenium In Situ Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Xenium data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html)

### 🎯 Objective

Analyzes **single-cell resolution** spatial transcriptomics data from the **10x Xenium In Situ** platform on human lung cancer tissue. Unlike spot-based Visium, Xenium assigns transcripts to individual segmented cells, enabling true single-cell spatial analysis.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Xenium In Situ |
| **Tissue** | Human Lung Cancer (NSCLC) |
| **FOVs** | 2 fields of view |
| **Download size** | ~8 GB |
| **Format** | Xenium output bundle → SpatialData Zarr |

### 🔬 Workflow

```
Download Xenium Human Lung Cancer bundle (~8 GB)
        │
        ▼
Parse with spatialdata-io xenium() reader
  └── Convert to SpatialData Zarr store
        │
        ▼
Extract AnnData table from SpatialData object
  └── obsm["spatial"] = (x, y) cell centroids
        │
        ▼
Quality Control
  ├── Total transcripts per cell
  ├── Unique genes per cell (n_genes_by_counts)
  ├── Cell area (µm²)
  ├── Nucleus-to-cell area ratio
  └── Control probe % / decoding error %
        │
        ▼
Filter (min_counts=10, min_cells=5)
Normalize → log1p → PCA → Neighbors → UMAP → Leiden
        │
        ▼
Build Spatial Neighborhood Graph (Delaunay triangulation)
        │
        ▼
Centrality Scores ──────► Cluster topology in graph
Co-occurrence Probability ► Cluster spatial range
Neighborhood Enrichment ─► Cluster co-localization (z-score)
Moran's I ───────────────► Spatially variable genes
        │
        ▼
Overlay results on morphology image
```

### 🖼️ Key Outputs

**1. QC distributions — four panel figure**

> Four histograms characterize the cell population: total transcript count (log-normal distribution expected), unique genes per cell, cell area in µm², and nucleus-to-cell area ratio. Low-quality cells cluster at the extreme left of the transcript count distribution.

![Xenium QC](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_11_0.png)

---

**2. UMAP — Leiden clusters, total counts, unique genes**

> The UMAP embedding at single-cell resolution. Each point is a segmented cell. Leiden clusters correspond to distinct lung cell populations: tumor cells, stromal fibroblasts, immune infiltrates (T cells, macrophages), and vascular endothelium.

![Xenium UMAP](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_16_0.png)

---

**3. Spatial scatter — single-cell cluster map**

> Each dot represents a single cell plotted at its (x, y) centroid in tissue space. At Xenium resolution, the spatial architecture of the tumor microenvironment becomes visible: tumor nests, stromal barriers, and immune infiltration zones.

![Xenium spatial](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_17_0.png)

---

**4. Centrality scores per cluster**

> Three graph-theoretic centrality measures (closeness, clustering coefficient, degree centrality) characterize how each Leiden cluster is positioned in the spatial connectivity graph. High closeness centrality clusters occupy central tissue positions; high clustering coefficient clusters form tight spatial niches.

![Centrality](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_20_0.png)

---

**5. Co-occurrence probability (cluster 12)**

> The co-occurrence curve for cluster 12 shows which other clusters become increasingly co-occurrent at larger spatial radii — identifying both immediate neighbors and longer-range associations in the tumor microenvironment.

![Co-occurrence Xenium](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_23_0.png)

---

**6. Neighborhood enrichment + spatial scatter (joint figure)**

> The z-score heatmap (left) reveals which cluster pairs significantly co-localize beyond chance; the spatial scatter (right) validates the enrichment visually. Strong enrichment between immune and stromal clusters highlights the tumor immune exclusion architecture.

![Nhood enrichment Xenium](https://squidpy.readthedocs.io/en/stable/_images/tutorial_xenium_27_0.png)

### 💡 Key Takeaways

- `spatialdata-io.xenium()` + Zarr is the recommended entry point — it preserves the full SpatialData object including transcript coordinates and morphology images
- Control probe % should be **< 1%** — anything higher indicates technical artifacts
- Delaunay triangulation (`coord_type='generic', delaunay=True`) is better than k-NN for cell-scale Xenium data because it respects the local geometry of the cell layout
- Subsample to 50% of cells before running co-occurrence (computationally expensive)

---

## 🔑 Key Concepts

<details>
<summary><b>📐 AnnData structure</b></summary>

All four notebooks use `AnnData` as the central data object:

```
AnnData
├── X               — (cells × genes) count matrix
├── obs             — per-cell metadata (QC metrics, cluster labels)
├── var             — per-gene metadata (highly_variable, mt flag)
├── obsm["spatial"] — (N × 2) spatial coordinates
├── obsm["X_pca"]   — PCA embedding
├── obsm["X_umap"]  — UMAP embedding
├── uns["spatial"]  — tissue image + scale factors (Visium)
└── obsp            — spatial neighborhood connectivity graph
```
</details>

<details>
<summary><b>🗺️ Spatial statistics explained</b></summary>

| Method | What it measures | Squidpy function |
|---|---|---|
| **Neighborhood enrichment** | z-score of cluster co-localization in kNN graph | `sq.gr.nhood_enrichment` |
| **Co-occurrence** | Probability that cluster B is near cluster A at radius r | `sq.gr.co_occurrence` |
| **Moran's I** | Global spatial autocorrelation of gene expression | `sq.gr.spatial_autocorr` |
| **Centrality scores** | Graph-theoretic position of clusters in spatial graph | `sq.gr.centrality_scores` |
| **Ligand-receptor** | Permutation-based L-R interaction significance | `sq.gr.ligrec` |
</details>

<details>
<summary><b>🏗️ Platform comparison</b></summary>

| Feature | Visium (H&E / Fluo) | MERFISH | Xenium In Situ |
|---|---|---|---|
| Resolution | ~55 µm spot | ~10 µm cell | Single-cell |
| Gene panel | Whole transcriptome | Targeted (~500) | Targeted (~300–500) |
| Image modality | H&E or fluorescence | FISH | H&E + IF |
| Data format | AnnData (sparse) | CSV + coord table | SpatialData Zarr |
| Segmentation | Spot grid | DAPI-based | Nucleus expansion |
</details>

---

## ⚙️ Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/spatial-transcriptomics-suite.git
cd spatial-transcriptomics-suite

# Create conda environment (recommended)
conda create -n spatial python=3.10
conda activate spatial

# Install core dependencies
pip install scanpy squidpy anndata spatialdata spatialdata-io spatialdata-plot
pip install seaborn leidenalg igraph ome-zarr openpyxl

# Launch Jupyter
jupyter lab
```

> ⚠️ **Xenium notebook**: requires ~8 GB disk space for the dataset download and ~16 GB RAM for full analysis. For a quick test, set `fraction=0.1` in the subsampling step.

---

## 🖼️ Results Gallery

| Notebook | Highlight |
|---|---|
| Scanpy Basics | Visium H&E with UMAP + spatial clusters + MERFISH embedding |
| Visium H&E | Neighborhood enrichment · Ligand-receptor · Moran's I SVGs |
| Visium Fluo | Watershed segmentation · Multi-scale morphology clustering |
| Xenium | Single-cell tumor microenvironment · Centrality · Co-occurrence |

---

## 📚 References & Tutorials

| Resource | Link |
|---|---|
| Scanpy spatial tutorial | https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html |
| Squidpy Visium H&E tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html |
| Squidpy Visium fluorescence tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html |
| Squidpy Xenium tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html |
| Squidpy paper | Palla *et al.*, *Nature Methods* 2022 |
| Scanpy paper | Wolf *et al.*, *Genome Biology* 2018 |
| SpatialData | Marconato *et al.*, *Nature Methods* 2024 |

---

<div align="center">

**Made with 🔬 and ☕ — spatial transcriptomics, one spot at a time.**

*Star ⭐ this repo if you found it useful!*

</div>