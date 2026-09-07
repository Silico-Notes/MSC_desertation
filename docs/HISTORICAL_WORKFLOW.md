# Historical Workflow

The original transcriptomic analysis for this cervical cancer study was executed on the [Galaxy web server](https://usegalaxy.org). The pipeline aimed to identify differentially expressed genes (DEGs) between cervical tumor samples and normal controls, and to subsequently interpret the results via functional enrichment and protein-protein interaction (PPI) networks.

## Provenance and Cohort

* **Tumor Dataset**: SRA Accession PRJNA521318. Historically stated as 8 SCC and 3 ADC samples (11 total).
* **Control Dataset**: SRA Accession PRJNA494155. Historically stated as 10 selected healthy control runs out of 19.
* **Confirmed Analysis Cohort**: 21 samples (11 Tumors, 10 Controls).

## Upstream Processing (Described in Thesis)

1. **Quality Check**: `FastQC` on raw sequencing reads.
2. **Read Trimming**: `Trim Galore` to remove adapters and poor-quality bases.
3. **Alignment**: `HISAT2` using a splice-aware mapping algorithm against the human reference genome (hg38).
4. **Quantification**: `StringTie` to calculate transcript/gene abundances and read counts.

## Differential Expression (Computational Evidence Recovered)

1. **Count Merging**: The 21 individual StringTie count tables were joined by column into a single merged raw count matrix.
2. **Differential Expression**: `DESeq2` (v1.22.1, R version 3.5.1) was used to compare the Tumor vs. Normal groups. 
3. **Filtering**: 
   - Adjusted P-value (FDR) $\le 1e-6$
   - Upregulated: $\log_2(	ext{Fold Change}) \ge 5$
   - Downregulated: $\log_2(	ext{Fold Change}) \le -10$
   - Gene Biotype: Filtered strictly for `protein_coding` genes.
4. **Output**: This produced a final list of 521 DEGs (313 upregulated, 208 downregulated). (Note: The thesis reports 523; this is an unresolved discrepancy).

## Downstream Analysis (Described in Thesis)

1. **Functional Enrichment**: `ShinyGO v0.66` was used for Gene Ontology (GO) and KEGG pathway enrichment (threshold $P < 0.05$).
2. **PPI Network**: The `STRING` database was queried for interactions among the DEGs, using a combined score cut-off $\ge 0.7$.
3. **Module Identification**: `Cytoscape` (v3.6.1) with the `MCODE` plug-in (Degree cutoff=2, Node score cutoff=0.2, k-core=2, max depth=100) identified hub genes, prominently CDK1, FN1, and ITGB1.
4. **Gene Set Enrichment Analysis (GSEA)**: `GSEA` (v4.0.3) was utilized against the MSigDB curated gene sets with 1000 permutations.

> **Note on Reproducibility**: While the upstream and differential expression analyses have been fully computationally validated and matched to historical output files, the downstream GUI-based analyses (ShinyGO, Cytoscape) lack strict computational provenance. The modernized workflow provided in this repository aims to replace these manual steps with programmatic, fully reproducible equivalents (e.g. `clusterProfiler`).
