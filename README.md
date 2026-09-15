# Spatial Transcriptomics Analysis: Human Brain Ageing

Seurat-based spatial transcriptomics analysis of human brain tissue (10X Genomics Visium) investigating transcriptional heterogeneity and spatial ageing signatures.

## Biological Questions

1. What transcriptionally distinct spatial domains exist in human brain tissue?
2. Where is senescence burden spatially concentrated?
3. Do skeletal muscle ageing DEGs show spatially restricted patterns in brain suggesting shared cross-tissue ageing programmes?

3. Do skeletal muscle ageing DEGs show spatially restricted 
   patterns in brain suggesting shared cross-tissue ageing 
   programmes?


## Dataset

| | |
|---|---|
| **Source** | 10X Genomics V1 Human Brain Section 1 |
| **Platform** | Visium spatial transcriptomics |
| **Spots** | ~4,900 capture spots |
| **Genes** | 36,601 |

## Methods

- **Normalisation:** SCTransform (Seurat v5)
- **Clustering:** PCA → KNN → Louvain (resolution 0.5)
- **Spatially variable genes:** Moran's I
- **Senescence scoring:** AddModuleScore (SenMayo signature)
- **Cross-tissue comparison:** muscle DEGs from the companion DESeq2 analysis (see below)

## Key Figures

### Figure 1 - Spatial Clusters: UMAP and Tissue
[![Clusters](https://github.com/VishCipher/spatial_transcriptomics_aging/raw/main/figures/01_umap_spatial_clusters.png)](/VishCipher/spatial_transcriptomics_aging/blob/main/figures/01_umap_spatial_clusters.png)

**What it is:** Two linked views of the same clustering result, a UMAP embedding (left/top, spots grouped by transcriptional similarity regardless of physical position) and the same cluster labels overlaid on the actual tissue image (right/bottom, spots colored by cluster at their real spatial coordinates).

**What it means:** If clusters that are transcriptionally similar in UMAP space also form contiguous, anatomically sensible regions on the tissue image, that's strong evidence the clustering is picking up real biological structure, distinct brain regions, rather than noise. This is the foundation every other figure in this analysis builds on.

### Figure 2 - Cell Type Marker Genes
[![Markers](https://github.com/VishCipher/spatial_transcriptomics_aging/raw/main/figures/02_marker_genes_spatial.png)](/VishCipher/spatial_transcriptomics_aging/blob/main/figures/02_marker_genes_spatial.png)

**What it is:** Spatial expression maps of known marker genes (e.g. MBP/MOBP for white matter/oligodendrocytes) plotted directly onto the tissue.

**What it means:** This is how the clusters in Figure 1 get biologically labeled. If a marker gene's expression lights up in exactly the region a cluster occupies, that cluster can be confidently annotated as that cell type or tissue structure, rather than left as an anonymous numbered cluster.

### Figure 3 - Spatial Senescence Burden
[![Senescence](https://github.com/VishCipher/spatial_transcriptomics_aging/raw/main/figures/03_senescence_score_spatial.png)](/VishCipher/spatial_transcriptomics_aging/blob/main/figures/03_senescence_score_spatial.png)

**What it is:** A per-spot senescence score (via Seurat's `AddModuleScore`, using the SenMayo senescence gene signature) plotted spatially across the tissue.

**What it means:** Rather than asking "is this tissue old or young" as one binary answer, this asks where within the tissue senescence-associated expression is concentrated. Uneven, region-restricted coloring (rather than uniform coloring everywhere) is the key thing to look for. It's the difference between "senescence is a general tissue property" and "senescence is localized to specific structures."

### Figure 4 - Cross-Tissue Ageing Signature
[![Cross-tissue](https://github.com/VishCipher/spatial_transcriptomics_aging/raw/main/figures/04_cross_tissue_aging.png)](/VishCipher/spatial_transcriptomics_aging/blob/main/figures/04_cross_tissue_aging.png)

**What it is:** The significant DEGs identified in the companion skeletal-muscle DESeq2 analysis, checked for detectability and spatial pattern in this brain dataset.

**What it means:** This is the cross-tissue question in Figure 3 above, made visual: do genes that changed with age in muscle also show a non-random spatial footprint in brain? Genes appearing here with clear spatial restriction (rather than uniform, ubiquitous expression) are candidates for a shared cross-tissue ageing programme, rather than tissue-specific, isolated effects.

### Figure 5 - Spatially Variable Genes
[![SVG](https://github.com/VishCipher/spatial_transcriptomics_aging/raw/main/figures/05_spatially_variable_genes.png)](/VishCipher/spatial_transcriptomics_aging/blob/main/figures/05_spatially_variable_genes.png)

**What it is:** Genes ranked by Moran's I, a statistic measuring spatial autocorrelation, how strongly a gene's expression clusters spatially rather than being scattered randomly across the tissue.

**What it means:** This is an unsupervised, hypothesis-free check: instead of looking at genes chosen in advance (like the markers in Figure 2), this asks the data "which genes organize themselves spatially at all?" Genes topping this list are candidates for further investigation independent of any prior assumption about what should be spatially patterned.

## Key Findings

- Clustering identified anatomically coherent brain regions
- MBP/MOBP expression clearly delineates white matter tracts
- Senescence burden shows spatial restriction across regions
- 11 of 13 skeletal muscle ageing DEGs are detectable in brain spatial data, with spatially non-random distribution

## Companion Analysis


See [deseq2-aging-analysis](https://github.com/VishCipher/deseq2-aging-analysis) for the bulk RNA-seq analysis whose DEGs are used in Figure 4 / the cross-tissue comparison above.


## How to Reproduce

Open `Spatial_trasncriptomics.Rmd` in RStudio and knit.

**Required packages:**

```r
install.packages("Seurat")
BiocManager::install("org.Hs.eg.db")
install.packages(c("tidyverse", "ggplot2", "patchwork"))
```

## Reference

10X Genomics Visium Human Brain Section 1 dataset. https://www.10xgenomics.com/datasets

## Author

**Vishishtaa Pandit** — [GitHub](https://github.com/VishCipher) · [LinkedIn](https://www.linkedin.com/in/vishishtaa-pandit28/)
