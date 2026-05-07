# ChIP-seq Analysis Pipeline for H3K36me3 in *Drosophila melanogaster*

## Overview

This project contains an R/Bioconductor workflow for analyzing ChIP-seq data from *Drosophila melanogaster* (dm6 genome assembly). The analysis focuses on the histone modification H3K36me3 on chromosome 3R.

The workflow includes:

* Loading the dm6 reference genome
* Retrieving gene annotations from Ensembl
* Visualizing transcripts using Gviz
* Importing ChIP-seq BED files
* Estimating fragment lengths
* Extending sequencing reads
* Binning the genome into fixed windows
* Counting read overlaps per genomic bin
* Visualizing enrichment tracks
* Importing and visualizing peak calls
* Identifying overlapping peaks between replicates
* Performing promoter overlap analysis

---

# Software and Packages

The analysis was performed in R using Bioconductor packages.

## Main packages used

```r
BiocManager
BSgenome.Dmelanogaster.UCSC.dm6
biomaRt
Gviz
GenomicRanges
IRanges
rtracklayer
chipseq
VennDiagram
```

---

# Genome Information

Organism:

```text
Drosophila melanogaster
```

Genome assembly:

```text
dm6 / bdgp6
```

Genome package:

```r
BSgenome.Dmelanogaster.UCSC.dm6
```

Gene annotations were retrieved from Ensembl BioMart.

---

# Input Files

The workflow expects the following BED files:

## ChIP/Input BED files

```text
GSE144244_H3K36Me3_Input_chr3R.bed
GSE144244_H3K36Me3_1.sorted_chr3R.bed
GSE144244_H3K36Me3_2.sorted_chr3R.bed
```

## Peak BED files

```text
H3K36me3_R1_peaks_chr3R.bed
H3K36me3_R2_peaks_chr3R.bed
```

These files contain aligned genomic intervals restricted to chromosome 3R.

---

# Directory Setup

It is recommended to avoid storing large genomics files inside OneDrive folders.

Example setup:

```text
D:\Genomics\BED
D:\Genomics\Plots
D:\Genomics\R_work
```

Set the working directory in R:

```r
setwd("D:/Genomics/R_work")
```

---

# Workflow Summary

## 1. Load Reference Genome

The dm6 reference genome is loaded using the BSgenome package.

```r
library(BSgenome.Dmelanogaster.UCSC.dm6)
```

This provides chromosome sequences and genome coordinate information.

---

## 2. Connect to Ensembl

Gene annotations are retrieved using biomaRt.

```r
library(biomaRt)
```

The Ensembl archive server is used for stability and reproducibility.

---

## 3. Create Gene Annotation Tracks

Gviz is used to generate transcript and genome annotation plots.

```r
library(Gviz)
```

The BiomartGeneRegionTrack function retrieves transcript structures including:

* Exons
* Introns
* UTRs
* Transcript isoforms

---

## 4. Import ChIP-seq BED Files

BED files are imported using:

```r
import.bed()
```

The imported data are stored as genomic ranges.

---

## 5. Fragment Extension

Sequencing reads are extended to estimated fragment lengths using:

```r
estimate.mean.fraglen()
resize()
```

This step approximates the actual DNA fragments pulled down during ChIP.

---

## 6. Genome Binning

Chromosome 3R is divided into 500 bp bins.

```r
tileGenome()
```

Read overlaps per bin are calculated using:

```r
countOverlaps()
```

This generates genomic enrichment profiles.

---

## 7. Coverage Visualization

Coverage tracks are visualized using Gviz DataTrack objects.

The resulting plots show:

* ChIP enrichment
* Input signal
* Gene annotations
* Transcript structures
* Peak locations

---

## 8. Peak Analysis

Peak BED files are imported and visualized.

Overlaps between replicate peaks are identified using:

```r
findOverlaps()
```

A Venn diagram is generated to compare replicate consistency.

---

## 9. Common Peak Identification

Shared peaks between replicates are identified using:

```r
Reduce(subsetByOverlaps, ...)
```

These represent higher-confidence enrichment regions.

---

## 10. Promoter Overlap Analysis

Promoter regions are defined around transcription start sites (TSS ± 200 bp).

Peak overlaps with promoters are then identified to determine whether H3K36me3 enrichment occurs near transcription initiation regions.

---

# Biological Interpretation

H3K36me3 is a histone modification commonly associated with:

* Active transcription
* Gene bodies
* Transcription elongation

The analysis helps identify genomic regions enriched for H3K36me3 and their relationship to annotated genes and promoters.

---

# Output Files

The workflow produces:

## bedGraph files

```text
Control_chr3R_bins500.bedGraph
H3K36me3_r1_chr3R_bins500.bedGraph
H3K36me3_r2_chr3R_bins500.bedGraph
```

## PDF plots

Genome browser-style visualizations generated using Gviz.

---

# Notes

* Large BAM files may require Linux systems or HPC resources.
* The current workflow operates primarily on processed BED files.
* Using chromosome-specific subsets (such as chr3R) significantly reduces memory usage.
* Avoid storing large sequencing files inside OneDrive-managed directories.

---

# Acknowledgements

This workflow was developed as part of an academic bioinformatics and epigenomics analysis project focused on ChIP-seq data interpretation and genomic feature analysis.
