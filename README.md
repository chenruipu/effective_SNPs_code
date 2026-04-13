# Functional SNP Identification and scRNA-STARR-seq Integration Analysis

## Overview

This repository contains the analysis scripts used in our study for:

* Identification of functional SNPs from sequencing data
* Comparison of different SNP detection strategies
* Visualization and statistical analysis
* Integration of SNP effects with single-cell RNA-seq data

The workflow combines bulk sequencing (STARR-seq / SNP counts) and single-cell transcriptomic data to characterize regulatory variants.


---

## Requirements

### Python

* python >= 3.8
* pandas
* numpy
* scanpy
* vcfpy

### R

* ggplot2
* dplyr
* DESeq2
* ggrepel
* VennDiagram
* ggsci

### Tools

* samtools (mpileup)

---

### 1️⃣ SNP Extraction and Processing (`python_scripts.ipynb`)

#### Step 1: Generate SNP counts from BAM files

```bash
samtools mpileup \
  -l all_snp.bed \
  -q 1 -t DP,AD -m 2 -F 0.002 \
  -uvf genome.fa \
  --output sample.vcf sample.bam
```

#### Step 2: Parse VCF and extract allele counts

* Extract REF / ALT counts per SNP
* Merge across samples
* Filter SNPs:

  * REF count > 0
  * ALT count valid
  * Remove multi-allelic sites

#### Step 3: SNP selection

* Compare **new vs old method**
* Calculate LFC differences
* Select:

  * top (high effect)
  * middle
  * low effect SNPs

#### Step 4: Generate BED file

* Extend ±50 bp around SNP
* Output for downstream analysis

---

### 2️⃣ Statistical Analysis & Visualization (`R_scripts.ipynb`)

#### Differential analysis

* Based on count matrices
* Using DESeq2-like framework
* Thresholds:

  * FDR < 0.05
  * |log2FC| > 0.585

#### Visualization

* Scatter plot (LFC vs significance)
* Volcano plot
* Correlation analysis

#### Method comparison

* Overlap between:

  * previous method
  * current method

→ Visualized using Venn diagram

#### Functional category analysis

* SNP functional categories:

  * enhancer ↔ silencer transitions
  * activity increase/decrease
* Pie chart visualization

---

### 3️⃣ scRNA-seq Integration (`scRNA_starr_seq.ipynb`)

#### Data loading

* 10x Genomics scRNA-seq data
* SNP annotation table

#### SNP–gene integration

* Match SNPs with:

  * STARR-seq results
  * scRNA-seq expressed genes

#### Cell filtering & QC

* min_genes filtering
* mitochondrial content filtering
* normalization & log transform

#### Dimensionality reduction

* PCA
* UMAP / t-SNE
* Leiden clustering

#### Marker gene identification

* Differential expression per cluster
* Top marker genes extraction

---

## Citation

If you use this code, please cite our paper (to be added).
