# Cervical Cancer RNA-seq Historical Portfolio

**This repository documents and organizes the computational biology work performed during my MSc thesis. It preserves the historical analysis workflow, computational outputs, figures, and research findings while providing provenance and documentation for the original project.**

## Biological Background and Research Objective
Cervical carcinomas (CC) are a leading cause of gynecological malignancies, with high-risk human papillomavirus (HPV) infection acting as a primary etiological factor. The research objective of this MSc thesis was to identify differentially expressed genes (DEGs) between human cervical tumor samples and healthy controls, aiming to discover genomic biomarkers, central hub genes, and critical pathways associated with disease progression.

## Original Analysis Workflow
The original transcriptomic analysis was executed as a sequential pipeline incorporating both command-line mapping and GUI-based network analysis:

`SRA` $\rightarrow$ `FastQC` $\rightarrow$ `Trim Galore` $\rightarrow$ `HISAT2` $\rightarrow$ `StringTie` $\rightarrow$ `DESeq2` $\rightarrow$ `Differential Expression` $\rightarrow$ `GO / KEGG` $\rightarrow$ `STRING` $\rightarrow$ `Cytoscape` $\rightarrow$ `MCODE` $\rightarrow$ `Hub Genes` $\rightarrow$ `GSEA`

*This represents the historical MSc workflow as actually performed; it is preserved here for authenticity rather than being silently replaced by modernized tools.*

## Historical Results
The core findings of the MSc thesis have been recovered and are preserved in the `results/historical/` directory, including:
* The final reported 523 Differentially Expressed Genes (314 upregulated, 209 downregulated).
* DESeq2 output plots (PCA, dispersion).
* Heatmaps generated during the analysis.

## Provenance and Forensic Notes
A rigorous computational recovery of the original workflow parameters was performed, ensuring high scientific honesty regarding the data:

* **Sample Cohort**: The documented thesis analysis cohort of 21 samples (11 Tumor + 10 Control) is successfully computationally validated. Two accessions from the broader public dataset (SRR8561801, SRR7946354) are absent from the historical analysis matrix; this remains an *unresolved discrepancy*.
* **DEG Thresholds**: Computational reconstruction confirms the exact historical filtering criteria: `padj <= 1e-6`, `|log2FC| >= 5` or `<= -10`, restricted to `protein_coding` genes.
* **Result Discrepancies**: The threshold application yields 521 genes. The thesis text reports 523. This difference of two genes remains an *unresolved discrepancy* and is intentionally preserved without fabrication.

For a detailed breakdown of the provenance, see the `docs/` folder.

## Repository Architecture
* `docs/`: Forensic provenance logs, the detailed historical workflow, and plans for future work.
* `results/historical/`: The surviving outputs, plots, and DEG lists from the original thesis workflow.

*Note: This repository is a historical portfolio. A full modern, automated reanalysis is outlined in `docs/FUTURE_WORK.md`.*
