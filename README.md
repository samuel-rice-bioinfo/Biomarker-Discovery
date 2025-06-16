# Biomarker-Discovery

## Overview

This repository contains all materials necessary to perform a principal component analysis (PCA) on RNA-seq expression profiles derived from mesenchymal stem cells (MSCs) differentiated into osteoblasts and tenocytes. The analysis aims to evaluate how chronological ageing influences transcriptional patterns in engineered musculoskeletal tissues. All preprocessing (alignment, normalization, filtering) has been completed prior to the present analysis; the files provided here focus on multivariate exploration using PCA.

## Repository Structure

```plaintext
├── README.md
├── LICENSE
├── LIFE752_Assessment1_data1F-1.csv
├── LIFE752_Assessment1_metadata1F-1.csv
└── Biomarker_Discovery.Rmd
```

* **README.md**
  Provides a scientific description of the project, instructions for installation and usage, and a summary of results and interpretations.
* **LICENSE**
  MIT License governing use of code and data; see for details.
* **LIFE752\_Assessment1\_data1F-1.csv**
  Filtered gene expression count matrix (rows = genes, columns = samples). Each entry represents a normalized read count.
* **LIFE752\_Assessment1\_metadata1F-1.csv**
  Sample metadata table. Each row corresponds to one sample and includes:

  * `SampleID` (matches column names in the expression matrix)
  * `Type` (differentiation outcome: “Osteoblast” or “Tenocyte”)
  * `Lab_tech` (laboratory technician: “Simon” or “Dave”)
* **Biomarker\_Discovery.Rmd**
  R Markdown script that performs the following:

  1. Loads required R packages.
  2. Reads expression and metadata files.
  3. Executes PCA on the transposed expression matrix (samples as observations, genes as variables), applying centering and scaling.
  4. Extracts principal component scores and computes the percentage of variance explained by PC1 and PC2.
  5. Produces PCA scatterplots colored by cell type and by lab technician.
  6. Provides a concise interpretation of both biological variation (cell type) and technical variation (batch effects).

## Background

Mesenchymal stem cells from young and aged donors were differentiated into osteoblasts and tenocytes. This experiment was conducted in collaboration between two laboratory technicians—Simon (≥10 years’ experience in RNA extraction) and Dave (6 months’ experience). The raw reads were aligned to the human reference genome, and a normalized count matrix was generated. To facilitate multivariate analysis, the dataset was filtered to include a subset of genes and has been normalized and scaled by the collaborating bioinformatics team. The objective of this PCA is to deconvolute the primary sources of variance—biological (cell type) and technical (lab technician)—and to detect any batch‐associated patterns.

## Prerequisites

* **R** (version ≥ 4.0.0)
* **R packages**:

  * `ggplot2` (for visualization)
  * `knitr` (for R Markdown rendering)
* Optional: **RStudio** or other IDE supporting R Markdown rendering

### Package Installation

Launch an R session and install any missing packages:

```r
install.packages(c("ggplot2", "knitr"))
```

## Usage

1. **Clone or Download the Repository**

   ```bash
   git clone https://github.com/<username>/Biomarker-Discovery.git
   cd Biomarker-Discovery
   ```

2. **Verify File Integrity**
   Ensure that the following files are present in the working directory:

   * `Biomarker_Discovery.Rmd`
   * `LIFE752_Assessment1_data1F-1.csv`
   * `LIFE752_Assessment1_metadata1F-1.csv`
   * `LICENSE`

3. **Render the R Markdown Report**
   In R or RStudio, execute:

   ```r
   rmarkdown::render("Biomarker_Discovery.Rmd")
   ```

   This produces `Biomarker_Discovery.html`, which contains:

   * Tables summarizing the first rows of expression and metadata files.
   * PCA scatterplots with samples colored by cell type and by lab technician.
   * Interpretation of results, including detection of batch effects and biological separation.

4. **Examine Results**

   * **PCA Plot Colored by Cell Type:**

     * PC1 accounts for the majority of variance (e.g., \~53%), demonstrating the transcriptional differences between osteoblasts and tenocytes.
     * Distinct clustering along PC1 indicates robust separation by differentiation status.
   * **PCA Plot Colored by Lab Technician:**

     * PC2 accounts for a secondary fraction of variance (e.g., \~23%), indicating a detectable batch effect associated with the individual who performed RNA extraction.
     * Batch effects are less pronounced than biological variation but warrant consideration (e.g., inclusion of `Lab_tech` as a covariate in downstream models).

## Data Description

* **Expression Matrix (`.csv`)**

  * Rows: Subset of genes selected for multivariate analysis.
  * Columns: Individual samples identified by unique `SampleID` values.
  * Entries: Normalized read counts (post‐alignment, library‐size correction, log or variance stabilization).

* **Metadata Table (`.csv`)**

  * Each row describes one sample.
  * Key columns:

    * `SampleID` (string): Matches the header of the corresponding column in the expression matrix.
    * `Type` (factor):

      * `"Osteoblast"`: MSCs differentiated into osteoblast lineage.
      * `"Tenocyte"`: MSCs differentiated into tenocyte lineage.
    * `Lab_tech` (factor):

      * `"Simon"`: Senior technician with extensive RNA extraction expertise.
      * `"Dave"`: Junior technician with six months of training.

## Summary of Findings

1. **Biological Variation (Cell Type)**

   * PC1 (≈ 53.74% variance explained) clearly separates osteoblast and tenocyte samples, reflecting major transcriptional differences induced by lineage specification.
   * Secondary variance along PC2 (≈ 23.08%) is attributed partly to cell‐type heterogeneity but is dominated by technical factors (see below).

2. **Technical Variation (Lab Technician)**

   * Samples processed by Simon and Dave cluster distinctly along PC2, confirming the presence of a batch effect.
   * The magnitude of this effect is smaller than the biological effect (PC2 < PC1), but it remains statistically meaningful and should be accounted for in downstream analyses (e.g., differential expression with batch covariate).

3. **Recommendations**

   * Include `Lab_tech` as a blocking factor or covariate in any differential expression or regression model to mitigate batch‐associated variance.
   * Consider alternative normalization or batch‐correction pipelines (e.g., ComBat, Limma) if further technical artefacts are suspected.

## File Descriptions

| File                                   | Description                                                                                              |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `LIFE752_Assessment1_data1F-1.csv`     | Normalized expression counts (filtered gene set). Rows = genes; columns = samples.                       |
| `LIFE752_Assessment1_metadata1F-1.csv` | Sample metadata. Columns include `SampleID`, `Type` (Osteoblast/Tenocyte), `Lab_tech` (Simon/Dave).      |
| `Biomarker_Discovery.Rmd`              | R Markdown script performing PCA and generating figures, with accompanying interpretation.               |
| `Biomarker_Discovery.html` (generated) | HTML report containing code output, figures, and narrative interpretation (product of rendering `.Rmd`). |
| `LICENSE`                              | MIT License.                                                                                             |
| `README.md`                            | This document.                                                                                           |

## Contributing

Contributions are welcome. To propose updates—whether modifications to code, addition of new analyses, or improvements to documentation—submit a pull request with a clear description of changes. All contributors should ensure reproducibility: any added code must run without error on a clean R installation with the specified packages.

## License

This project is licensed under the MIT License. See the `LICENSE` file for full terms.

## References

1. **Jolliffe, I. T.** (2002). *Principal Component Analysis*. Springer Series in Statistics.
2. **Leek, J. T., Johnson, W. E., Parker, H. S., Jaffe, A. E., & Storey, J. D.** (2010). Removing unwanted variation from high-dimensional data with negative controls. *Biostatistics*, 11(4), 539–552.
3. **Love, M. I., Huber, W., & Anders, S.** (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*, 15, 550.

---

*This README was generated to provide comprehensive, reusable documentation for the Biomarker Discovery PCA pipeline.*
