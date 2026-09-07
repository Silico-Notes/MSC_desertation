# Forensic Inventory

## Files Found in `SOURCE_MATERIAL`

- **`desertation.pdf`**: Primary historical document describing the workflow. Contains the explicitly stated sample sizes of 11 Tumors (8 SCC, 3 ADC) and 10 Controls.
- **`19378022-SameerDahale.pdf`**: Secondary historical document.
- **`deseq-data.tar.gz`**: A Galaxy history archive containing intermediate outputs:
  - 21 individual StringTie gene counts (`.tabular`).
  - Merged count matrix for all 21 samples (`Column Join on data 28_ data 27_ and others.tabular`).
  - DESeq2 outputs (plots, results, normalized matrices).
- **Galaxy outputs** (standalone in `SOURCE_MATERIAL`):
  - `Galaxy640-[DESeq2 result file on data 627, data 622, and others].tabular`
  - `Galaxy641-[DESeq2 plots on data 627, data 622, and others].pdf`
  - `Galaxy642-[Normalized counts file on data 627, data 622, and others].tabular`
  - `Galaxy643-[rLog-Normalized counts file on data 627, data 622, and others].tabular`
  - `Galaxy644-[VST-Normalized counts file on data 627, data 622, and others].tabular`
  - `Galaxy645-[heatmap2 on data 640].pdf`
  - `Galaxy648-[TOTAL_DEG.csv].csv` (Contains subset of DESeq2 results, likely the 523 DEGs)
  - `Galaxy652-[heatmap2 on data 650].pdf`
  - `Galaxy653-[Charts on data 650].tabular`
  - `Galaxy654-[heatmap2 on data 650].pdf`

## Datasets and Samples Extracted

### 1. Count Matrix (DESeq2 Input)
Found in `deseq-data.tar.gz` as `Column Join on data 28_ data 27_ and others.tabular`. This is the combined read count matrix.

### 2. Sample Identifiers (21 total)
The column matrix successfully maps to exactly 21 samples.
**Tumor (11 samples):**
- SRR8561802
- SRR8561803
- SRR8561804
- SRR8561805
- SRR8561806
- SRR8561807
- SRR8561808
- SRR8561809
- SRR8561810
- SRR8561811
- SRR8561812

**Control (10 samples):**
- SRR7946355
- SRR7946356
- SRR7946357
- SRR7946358
- SRR7946359
- SRR7946360
- SRR7946361
- SRR7946362
- SRR7946363
- SRR7946364

**Excluded Samples (Discrepancy resolved)**:
- SRR8561801 (Tumor)
- SRR7946354 (Control)
As confirmed by the methodology section of `desertation.pdf`, these samples were explicitly not selected for analysis.

## Parameters & Metadata Recovered
From `datasets_attrs.txt` in Galaxy archive:
- **Reference Genome**: `hg38`
- **DESeq2 version**: `1.22.1` (R version `3.5.1`)
- **Number of genes**: `28,277` (in result file) to `28,279` (in matrix)

## Provenance Chain
1. SRA -> FASTQ: Supported by SRA accessions (Tumor: PRJNA521318, Control: PRJNA494155).
2. Quality check (FastQC) & Trimming (Trim-Galore) -> Alignment (HISAT2): Reported in thesis, no intermediate files found yet.
3. Alignment -> StringTie Gene Counts: **Recovered direct computational evidence** (21 StringTie `.tabular` files).
4. StringTie -> Count Matrix: **Recovered direct computational evidence** (Joined `.tabular` file mapping directly to the 21 outputs).
5. Count Matrix -> DESeq2: **Recovered direct computational evidence** (Outputs, plots, and run info matching).

## DEG Filtering Criteria
Recovered from TOTAL_DEG.csv and confirmed by thesis text:
- **Adjusted P-value (FDR)**: <= 1e-6
- **Upregulated**: log2FC >= 5
- **Downregulated**: log2FC <= -10
This produces exactly 521 genes (313 up, 208 down) in the recovered file, matching the reported 523 within a margin of 2, likely due to counting errors in the original report or Excel headers.

**Correction to Excluded Samples**:
- The exclusion of SRR8561801 and SRR7946354 is NOT definitively proven to be intentional. While the thesis states 11 tumor and 10 control samples were selected, it does not name the specific excluded runs. The reason for their exclusion remains UNKNOWN/UNRESOLVED.

## Rigorous DEG Threshold Reconstruction
- **Raw Rows**: The DESeq2 result contains 28,278 genes.
- **Mathematical Thresholds**: `padj <= 1e-6`, Upregulated `log2FC >= 5`, Downregulated `log2FC <= -10`. Applying these yields 716 DEGs (496 up, 220 down).
- **Annotation Filter**: The `TOTAL_DEG.csv` file adds a `Gene.type` column, and every entry is `protein_coding`. This secondary filter reduces the 716 DEGs down to the 521 DEGs found in the file.
- **Reporting Discrepancy**: The thesis reports 523 DEGs (314 up, 209 down), which is exactly 2 off from the 521 DEGs (313 up, 208 down) in the recovered file. This is an unresolved discrepancy unless direct evidence proves the cause.

## Downstream Analyses Recovered Parameters

### Functional Enrichment Analysis (GO / KEGG)
- **Software**: ShinyGO v0.66 (via shiny package in R)
- **Input Gene List**: The filtered DEG list (presumably the 521/523 DEGs, though thesis may have used upregulated and downregulated separately).
- **Threshold**: P < 0.05.

### Protein-Protein Interaction (PPI) Network
- **Database**: STRING database (http://www.bork.embl.heidelberg.de/STRING/)
- **Parameter**: Combined score cut-off $\ge 0.7$ (High confidence).
- **Reported Result**: Expected number of edges: 282, PPI enrichment p-value: < 1.0e-16. Total nodes: 318, Number of edges: 1212.

### Module Identification & Hub Genes
- **Software**: Cytoscape (version 3.6.1) with MCODE plug-in.
- **Parameters**: 
  - Degree cutoff = 2
  - Node score cutoff = 0.2
  - k-core = 2
  - Max depth = 100
- **Reported Result**: 317 hub genes were identified by cytoscape overall, but "top 10 genes for each method were extracted. A total of 100 genes that appeared at least twice were conserved as hub genes". Then mentions "A total of 29 genes were identified... Followed by that A total of 25 hub genes were verified".
- **Major Hub Genes**: CDK1 (degree=16), FN1 (degree=12), ITGB1 (degree=8).

### GSEA
- **Software**: GSEA software version 4.0.3.
- **Database**: MSigDB curated gene sets.
- **Parameters**: 1000 gene set permutations.
