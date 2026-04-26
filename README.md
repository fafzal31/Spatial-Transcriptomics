<div align="center">

# 🧬 Spatial Transcriptomics Analysis Suite

### End-to-end workflows for Visium · MERFISH · Xenium using Scanpy & Squidpy

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)](https://python.org)
[![Scanpy](https://img.shields.io/badge/Scanpy-1.10%2B-orange?style=for-the-badge)](https://scanpy.readthedocs.io)
[![Squidpy](https://img.shields.io/badge/Squidpy-1.4%2B-green?style=for-the-badge)](https://squidpy.readthedocs.io)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-F37626?style=for-the-badge&logo=jupyter)](https://jupyter.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

<br/>

> *Decoding the spatial language of cells — where gene expression meets tissue architecture.*

</div>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Repository Structure](#-repository-structure)
- [Notebook 1 — Scanpy Spatial Basics: Visium + MERFISH](#-notebook-1--scanpy-spatial-basics-visium--merfish)
- [Notebook 2 — Visium H&E Analysis with Squidpy](#-notebook-2--visium-he-analysis-with-squidpy)
- [Notebook 3 — Visium Fluorescence Analysis with Squidpy](#-notebook-3--visium-fluorescence-analysis-with-squidpy)
- [Notebook 4 — Xenium In Situ Analysis with Squidpy](#-notebook-4--xenium-in-situ-analysis-with-squidpy)
- [Key Concepts](#-key-concepts)
- [Installation](#-installation)
- [References](#-references)

---

## 🔭 Overview

This repository contains four end-to-end spatial transcriptomics analysis notebooks covering the most important spatial omics platforms — **10x Visium**, **MERFISH**, and **10x Xenium**. Each notebook follows an official tutorial from the Scanpy / Squidpy ecosystem and extends it with a complete, reproducible workflow.

| Step | Description |
|---|---|
| 📥 **Data loading** | Platform-specific loaders (AnnData, SpatialData, Zarr) |
| 🔬 **Quality control** | Per-cell metrics, mitochondrial content, control probes |
| ⚙️ **Preprocessing** | Normalization, log-transform, HVG selection |
| 📊 **Dimensionality reduction** | PCA → k-NN graph → UMAP |
| 🔵 **Clustering** | Leiden community detection |
| 🗺️ **Spatial statistics** | Neighborhood enrichment, co-occurrence, Moran's I |
| 🧪 **Image analysis** | Morphology features, watershed cell segmentation |
| 💬 **Cell communication** | Ligand–receptor interaction screening |

---

## 📂 Repository Structure

```
📦 spatial-transcriptomics-suite/
├── 📓 Analysis_and_visualization_of_spatial_transcriptomics_data.ipynb
├── 📓 Analyze_Visium_H_E_data.ipynb
├── 📓 Analyze_Visium_fluorescence_data.ipynb
├── 📓 Analyze_Xenium_Data.ipynb
├── 📁 images/                  ← all output figures from the notebooks
└── 📄 README.md
```

---

## 📓 Notebook 1 — Scanpy Spatial Basics: Visium + MERFISH

> **Tutorial:** [Scanpy — Analysis and visualization of spatial transcriptomics data](https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html)

### 🎯 Objective

Introduces the foundational Scanpy spatial workflow on a **10x Visium Human Lymph Node** section, then extends the same pipeline to **MERFISH** data — demonstrating that the Scanpy API is platform-agnostic.

### 📚 Datasets

| Property | Visium | MERFISH |
|---|---|---|
| **Tissue** | Human Lymph Node | Mouse hypothalamus |
| **Resolution** | ~55 µm spots | ~10 µm cells |
| **Format** | `sc.datasets.visium_sge` | CSV counts + XLS coordinates |

### 🔬 Analysis Pipeline

```
Load Visium SGE  →  QC metrics  →  Filter (MT < 20%, counts 5k–35k)
→  Normalize & log1p  →  Top 2000 HVGs  →  PCA → Neighbors → UMAP
→  Leiden clustering  →  Spatial visualization  →  Marker gene heatmap
──── MERFISH extension ────
CSV counts + coordinate table → AnnData → same pipeline → spatial embedding
```

---

### 📊 Results

**Quality Control — Distribution of UMI counts and detected genes per spot**

> Four histograms showing `total_counts` (full range and zoomed to < 10k) and `n_genes_by_counts` (full range and zoomed to < 4k). The roughly bell-shaped UMI distribution centered near 20,000 counts indicates healthy tissue coverage. The zoomed panels help set accurate filtering thresholds.

![QC histograms](images/scanpy_basic_00.png)

---

**UMAP Embedding — colored by total counts, gene counts, and Leiden clusters**

> Ten Leiden clusters separate cleanly in the UMAP. Notably, count-based coloring (left two panels) does not recapitulate the cluster structure — confirming that the clustering reflects true transcriptional diversity rather than sequencing depth artifacts.

![UMAP clusters](images/scanpy_basic_01.png)

---

**Spatial scatter — QC metrics overlaid on H&E tissue image**

> Total counts and gene counts projected back onto the Visium capture area. High-count (yellow) spots localize to the densely cellular germinal center and mantle zone regions of the lymph node — the morphological architecture is immediately apparent even before clustering.

![Spatial QC](images/scanpy_basic_02.png)

---

**Spatial scatter — Leiden clusters on tissue**

> All 10 Leiden clusters mapped onto the lymph node section. Contiguous spatial domains correspond to distinct immunological compartments: B-cell follicles, T-cell zones, stromal regions, and the subcapsular sinus.

![Spatial clusters](images/scanpy_basic_03.png)

---

**Cropped region — highlighting clusters 5 and 9**

> A focused crop of the tissue reveals the precise localization of clusters 5 and 9 (brown and light blue) along the outer capsule and sinusoidal spaces of the lymph node, facilitating targeted biological interpretation.

![Cropped region](images/scanpy_basic_04.png)

---

**Marker gene heatmap — top 10 DEGs for cluster 9 vs all clusters**

> t-test differential expression ranks genes for cluster 9. Top markers include **CCL21** (lymph node stromal marker), **SPARCL1**, **VWF** (vascular endothelium), and **ENG** — confirming this cluster corresponds to lymphatic/vascular endothelial cells.

![Heatmap cluster 9](images/scanpy_basic_05.png)

---

**Spatial gene expression — cluster identity vs. CR2 expression**

> **CR2** (Complement Receptor 2 / CD21), a canonical B-cell marker, co-localizes precisely with the follicular B-cell region — validating the biological identity of the transcriptionally defined clusters.

![CR2 spatial](images/scanpy_basic_06.png)

---

**Spatial gene expression — COL1A2 and SYPL1**

> Spatial mapping of **COL1A2** (collagen type I, a stromal/fibroblast marker) and **SYPL1** reveals complementary expression patterns — COL1A2 enriched in the perifollicular stroma, SYPL1 showing a diffuse distribution across follicular regions.

![Gene expression spatial](images/scanpy_basic_07.png)

---

**MERFISH — UMAP embedding (7 clusters)**

> The same Scanpy pipeline applied to MERFISH data yields 7 distinct cell clusters from mouse hypothalamus tissue. The UMAP topology is sparser than Visium (fewer cells) but cluster separation is clear.

![MERFISH UMAP](images/scanpy_basic_08.png)

---

**MERFISH — Spatial embedding (cell coordinates in µm)**

> Unlike spot-based Visium, MERFISH provides continuous (x, y) coordinates in microns for each cell. The spatial embedding shows that clusters do not form strongly exclusive spatial domains in this hypothalamic region — reflecting the intermingled nature of hypothalamic cell types.

![MERFISH spatial](images/scanpy_basic_09.png)

### 💡 Key Takeaways

- `sc.pl.spatial()` overlays any `.obs` column directly on the tissue image — works identically for Visium and MERFISH
- The Scanpy recipe (normalize → log1p → HVG → PCA → neighbors → UMAP → Leiden) is fully platform-agnostic
- Marker gene discovery via `sc.tl.rank_genes_groups` + spatial projection enables rapid biological annotation of clusters

---

## 📓 Notebook 2 — Visium H&E Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Visium H&E data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html)

### 🎯 Objective

Goes beyond visualization to perform rigorous **spatial statistics** on a mouse brain coronal Visium section — demonstrating how morphological image features and graph-based spatial analysis complement gene-expression clustering.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Visium |
| **Tissue** | Mouse Brain (coronal section) |
| **Stain** | Hematoxylin & Eosin (H&E) |
| **Annotation** | 15 anatomical regions (Cortex 1–5, Hippocampus, Thalamus, etc.) |

### 🔬 Analysis Pipeline

```
Load pre-processed Visium H&E AnnData + ImageContainer
→  Spatial scatter of annotated gene-expression clusters
→  Image feature extraction (summary statistics, scale 1.0 & 2.0)
→  Feature-based Leiden clustering → compare with gene clusters
→  Spatial neighborhood graph (sq.gr.spatial_neighbors)
→  Neighborhood enrichment  →  Co-occurrence  →  Ligand-receptor  →  Moran's I
```

---

### 📊 Results

**Spatial scatter — annotated brain regions on H&E tissue**

> 15 annotated brain regions projected onto the Visium H&E section. Cortical layers, Hippocampal subfields, Hypothalamic nuclei, and Thalamic regions are clearly resolved at spot resolution — the foundational reference for all downstream spatial statistics.

![H&E scatter](images/visium_hne_00.png)

---

**Image feature clustering vs. gene-expression clustering**

> Left: Leiden clustering derived purely from H&E image summary features (mean, std, quantiles at 2 scales). Right: transcriptomic Leiden clusters with anatomical annotations. The strong spatial concordance between the two independent modalities validates that tissue morphology and transcriptomics carry the same biological signal.

![Feature vs gene clusters](images/visium_hne_01.png)

---

**Neighborhood enrichment heatmap**

> z-score matrix of cluster co-localization across all 15 annotated brain regions. The bright diagonal confirms strong self-enrichment (anatomically compact regions). Off-diagonal enrichment between Hippocampus and Fiber_tract reflects their anatomical adjacency; Cortex layers show graded enrichment with neighboring layers but depletion from deep subcortical regions.

![Neighborhood enrichment](images/visium_hne_02.png)

---

**Co-occurrence probability — source cluster: Hippocampus**

> Probability of observing each target cluster at increasing spatial radii from any Hippocampus spot. Pyramidal_layer and Pyramidal_layer_dentate_gyrus show high short-range co-occurrence (< 500 µm), consistent with their anatomical proximity. All curves converge toward 1.0 at large distances (tissue-level baseline).

![Co-occurrence](images/visium_hne_03.png)

---

**Spatially variable genes — Olfm1, Plp1, Itpka + cluster reference**

> The top 3 genes by Moran's I spatial autocorrelation: **Olfm1** (enriched in hippocampal CA fields), **Plp1** (myelin proteolipid protein, strongly enriched along Fiber_tract), and **Itpka** (enriched in Striatum and Cortex). Each gene's spatial expression pattern directly mirrors the anatomical annotation on the right.

![Spatially variable genes](images/visium_hne_05.png)

### 💡 Key Takeaways

- Computing image features at **two scales** (1.0 = spot-level, 2.0 = tissue-context) substantially improves morphology-based clustering
- Neighborhood enrichment provides a quantitative, permutation-backed measure of anatomical co-localization — superior to visual inspection alone
- Moran's I top hits are known region-specific markers, validating the approach end-to-end

---

## 📓 Notebook 3 — Visium Fluorescence Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Visium fluorescence data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html)

### 🎯 Objective

Demonstrates **cell segmentation from multi-channel immunofluorescence images** and extraction of morphological features per spot — adding a protein-level layer to transcriptomic analysis and enabling comparison of three independent morphological feature sets.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Visium |
| **Tissue** | Mouse Brain (cropped section) |
| **Imaging** | 3-channel fluorescence (DAPI + 2 markers) |
| **Loader** | `sq.datasets.visium_fluo_adata_crop()` |

### 🔬 Analysis Pipeline

```
Load cropped fluorescence Visium + 3-channel ImageContainer
→  Visualize individual channels (channelwise=True)
→  Gaussian smoothing  →  Watershed segmentation (DAPI channel)
→  Extract segmentation features (cell count, channel intensities)
→  Multi-scale feature extraction:
      features_orig    (summary + texture + histogram, scale=1.0, mask_circle=True)
      features_context (summary + histogram, scale=1.0)
      features_lowres  (summary + histogram, scale=0.25)
→  Leiden clustering per feature set  →  compare with gene clusters
```

---

### 📊 Results

**Spatial scatter — gene-expression clusters on fluorescence tissue**

> Annotated transcriptomic clusters overlaid on the fluorescence image. Distinct brain structures — Cortex layers, Hippocampus (the distinctive curved arc), Fiber tracts, and Thalamus — are clearly visible both in the fluorescence image and as spatially coherent clusters.

![Fluorescence scatter](images/visium_fluo_00.png)

---

**Multi-channel fluorescence image (channelwise display)**

> The three raw fluorescence channels shown independently. Channel 0 (left, bright foci) corresponds to DAPI-stained nuclei with high signal in the hippocampal pyramidal layer arc. Channels 1 and 2 show progressively sparser, more specific staining patterns highlighting distinct cellular populations.

![Fluorescence channels](images/visium_fluo_01.png)

---

**Watershed segmentation — raw image vs. segmentation mask**

> A 500×500 µm crop showing individual cells before (left) and after (right) watershed segmentation. Each colored instance in the mask corresponds to one segmented cell. The segmentation correctly identifies densely packed cells in the pyramidal layer and sparser cells in adjacent regions.

![Watershed segmentation](images/visium_fluo_02.png)

---

**Segmentation-derived feature maps**

> Per-spot aggregated segmentation features mapped onto the tissue. **Top-left**: `segmentation_label` (cell count per spot) — peaks in the densely packed pyramidal layer arc. **Top-right**: gene-expression cluster reference. **Bottom-left/right**: mean intensity of channels 0 and 1 per spot — the Hippocampus arc lights up strongly in channel 0, confirming DAPI's nuclear staining of the densely nucleated pyramidal cells.

![Segmentation features](images/visium_fluo_03.png)

---

**Multi-feature Leiden clustering comparison**

> Three independent image-derived clusterings (summary features, histogram features, texture features) compared to the gene-expression reference (bottom-left). All three image-based clusterings recover the Hippocampus arc and broad cortical layers — validating that fluorescence morphology carries substantial biological information even without any gene expression data.

![Feature clustering comparison](images/visium_fluo_04.png)

### 💡 Key Takeaways

- `mask_circle=True` restricts feature extraction to the physical Visium spot footprint — essential for accurate per-spot quantification
- **Texture features** detect microarchitectural patterns (cell packing density, nuclear size variability) that summary and histogram features miss
- The concordance between morphology-based and transcriptomics-based clusters confirms that spatial multi-omics integration is biologically grounded

---

## 📓 Notebook 4 — Xenium In Situ Analysis with Squidpy

> **Tutorial:** [Squidpy — Analyze Xenium data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html)

### 🎯 Objective

Analyzes **single-cell resolution** spatial transcriptomics from the **10x Xenium In Situ** platform on human lung cancer tissue. Unlike spot-based Visium, Xenium assigns transcripts to individual segmented cells — enabling true single-cell spatial analysis and high-resolution mapping of the tumor microenvironment.

### 📚 Dataset

| Property | Details |
|---|---|
| **Platform** | 10x Genomics Xenium In Situ v2 |
| **Tissue** | Human Lung Cancer (NSCLC), 2 FOVs |
| **Gene panel** | ~300 targeted genes |
| **Download** | ~8 GB (Xenium output bundle) |
| **Format** | SpatialData Zarr store |

### 🔬 Analysis Pipeline

```
Download Xenium bundle  →  Parse with spatialdata-io xenium()
→  Write to Zarr  →  Extract AnnData table (obsm["spatial"] = cell centroids)
→  QC: transcripts/cell, genes/cell, cell area, nucleus ratio, control probe %
→  Filter (min_counts=10, min_cells=5)
→  Normalize → log1p → PCA → Neighbors → UMAP → Leiden
→  Build Delaunay spatial graph (sq.gr.spatial_neighbors)
→  Centrality scores → Co-occurrence → Neighborhood enrichment → Moran's I
→  Overlay spatially variable genes on morphology image
```

---

### 📊 Results

**Quality control — four-panel cell characterization**

> QC histograms at single-cell resolution: **total transcripts per cell** (log-normal, peak ~50 counts), **unique genes per cell** (peak ~25), **cell area** (µm², right-skewed indicating heterogeneous cell sizes), and **nucleus-to-cell area ratio** (broad distribution centered ~0.45). Control probe % was << 1%, confirming high assay specificity with minimal background.

![Xenium QC](images/xenium_00.png)

---

**UMAP — single-cell embedding with 13 Leiden clusters**

> Each point is a single segmented cell. The UMAP reveals 13 clusters with a large central cloud (mixed epithelial/stromal cells) and a distinct separated cluster 12 (pink, bottom right) — a rare population with unique transcriptional identity, likely a specialized immune or tumor cell type.

![Xenium UMAP](images/xenium_01.png)

---

**Spatial scatter — single-cell cluster map (full FOV)**

> Every cell plotted at its physical (x, y) centroid within lung tissue. At Xenium single-cell resolution, the **tumor microenvironment** architecture is visible: dense tumor cell nests, stromal barriers, and scattered immune infiltrates. Cluster 12 (pink) is spatially restricted to specific focal regions in the upper-right portion of the FOV.

![Xenium spatial full](images/xenium_02.png)

---

**Centrality scores — three graph-theoretic measures per cluster**

> Three complementary centrality measures from the Delaunay spatial neighborhood graph: **Average clustering** (how tightly cells group), **Closeness centrality** (how centrally positioned in tissue), and **Degree centrality** (proportion of cross-cluster connections). Cluster 0 ranks highest across all three — indicating a spatially central, highly connected population consistent with a major stromal or epithelial compartment.

![Centrality scores](images/xenium_03.png)

---

**Co-occurrence probability — source cluster: 12**

> Cluster 12 shows dramatically elevated self-co-occurrence (~16×) at very short distances (< 100 µm), rapidly decaying to baseline by 300 µm. This sharp short-range signature indicates cluster 12 cells form tight focal aggregates — consistent with a rare cell type forming organized microanatomical structures such as tertiary lymphoid structures or tumor cell clusters.

![Co-occurrence xenium](images/xenium_04.png)

---

**Subsampled spatial scatter — 50% of cells used for co-occurrence**

> The 50% subsample preserves the overall tissue architecture and cluster distribution faithfully — validating its use for the computationally intensive co-occurrence and Moran's I calculations.

![Xenium subsample scatter](images/xenium_05.png)

---

**Neighborhood enrichment + spatial reference**

> **Left**: z-score heatmap — cluster 12 shows strong self-enrichment (z > 100, yellow) and depletion with most other clusters, quantitatively confirming its spatially isolated nature. Most other clusters show moderate mutual enrichment, reflecting the mixed cellular composition of lung tumor stroma. **Right**: spatial reference scatter for the subsampled dataset.

![Neighborhood enrichment xenium](images/xenium_06.png)

---

**Spatially variable genes — AREG and MET expression**

> Top genes by Moran's I spatial autocorrelation: **AREG** (amphiregulin, an EGFR ligand frequently overexpressed in NSCLC) and **MET** (hepatocyte growth factor receptor, a known lung cancer oncogene and therapeutic target). Both show sparse but spatially clustered expression — consistent with focal tumor cell populations expressing cancer driver genes at specific anatomical locations.

![Spatially variable genes Xenium](images/xenium_07.png)

### 💡 Key Takeaways

- `spatialdata-io.xenium()` + Zarr is the canonical entry point — preserving the full SpatialData object including transcript coordinates and morphology images
- **Delaunay triangulation** (`coord_type='generic', delaunay=True`) is preferable to k-NN for Xenium because it respects local cell layout geometry without requiring a fixed k
- Control probe % must be < 1% — higher values indicate transcript misassignment artifacts
- Moran's I top hits (AREG, MET) are clinically actionable lung cancer genes — demonstrating how spatial autocorrelation directly guides biological and translational discovery

---

## 🔑 Key Concepts

<details>
<summary><b>📐 AnnData object structure</b></summary>

All four notebooks store data in the `AnnData` format:

```
AnnData
├── X                 — (cells/spots × genes) count matrix (sparse)
├── obs               — per-cell metadata: QC metrics, cluster labels
├── var               — per-gene metadata: highly_variable flag, mt flag
├── obsm["spatial"]   — (N × 2) spatial coordinates (µm or pixel)
├── obsm["X_pca"]     — PCA embedding
├── obsm["X_umap"]    — UMAP embedding
├── uns["spatial"]    — tissue image + scale factors (Visium only)
└── obsp              — spatial connectivity/distance matrices
```
</details>

<details>
<summary><b>🗺️ Spatial statistics methods explained</b></summary>

| Method | What it measures | Function |
|---|---|---|
| **Neighborhood enrichment** | z-score of cluster co-localization in spatial graph | `sq.gr.nhood_enrichment` |
| **Co-occurrence** | Probability cluster B is near cluster A at radius r | `sq.gr.co_occurrence` |
| **Moran's I** | Global spatial autocorrelation of gene expression | `sq.gr.spatial_autocorr` |
| **Centrality scores** | Graph-theoretic position of clusters | `sq.gr.centrality_scores` |
| **Ligand-receptor** | Permutation-based L-R interaction significance | `sq.gr.ligrec` |
| **Image features** | Summary/texture/histogram statistics per spot | `sq.im.calculate_image_features` |
</details>

<details>
<summary><b>🏗️ Platform comparison</b></summary>

| Feature | Visium (H&E / Fluo) | MERFISH | Xenium In Situ |
|---|---|---|---|
| **Resolution** | ~55 µm spot (bulk) | ~10 µm (single-cell) | Single-cell |
| **Gene panel** | Whole transcriptome | Targeted (~500) | Targeted (~300) |
| **Image** | H&E or fluorescence | FISH image | H&E + IF |
| **Data format** | AnnData (sparse) | CSV + coordinates | SpatialData Zarr |
| **Cell count** | ~3k–5k spots | ~1k–10k cells | ~10k–100k cells |
</details>

---

## ⚙️ Installation

```bash
# Clone the repository
git clone https://github.com/fafzal31/Spatial-Transcriptomics
cd spatial-transcriptomics-suite

# Create a dedicated environment (recommended)
conda create -n spatial python=3.10
conda activate spatial

# Install all dependencies
pip install scanpy squidpy anndata
pip install spatialdata spatialdata-io spatialdata-plot ome-zarr
pip install seaborn leidenalg igraph openpyxl

# Launch Jupyter
jupyter lab
```

> ⚠️ **Xenium notebook**: requires ~8 GB disk space for dataset download and ~16 GB RAM. For a quick test, set `fraction=0.1` in the subsampling step.

---

## 📚 References

| Resource | Link |
|---|---|
| Scanpy spatial tutorial | https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html |
| Squidpy Visium H&E tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html |
| Squidpy Visium fluorescence tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html |
| Squidpy Xenium tutorial | https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html |
| Squidpy paper | Palla *et al.*, *Nature Methods* 2022 |
| Scanpy paper | Wolf *et al.*, *Genome Biology* 2018 |
| SpatialData paper | Marconato *et al.*, *Nature Methods* 2024 |

---

<div align="center">

**Made with 🔬 and ☕ — spatial transcriptomics, one spot at a time.**

*⭐ Star this repo if you found it useful!*

</div>