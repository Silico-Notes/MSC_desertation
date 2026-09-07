# Forensic Inventory and Reconstruction Summary

This document addresses the 12 primary questions of the forensic reconstruction objective based on the evidence recovered.

## 1. What files exist?
All surviving historical files are located in `SOURCE_MATERIAL/`. This includes two copies of the thesis (`desertation.pdf`, `19378022-SameerDahale.pdf`), a Galaxy history archive (`deseq-data.tar.gz`), and several standalone Galaxy outputs (DESeq2 results, plots, heatmaps, and a DEG list).

## 2. What datasets exist?
- 21 individual `StringTie` gene count files.
- 1 merged gene count matrix (`Column Join on data 28_ data 27_ and others.tabular`).
- Full DESeq2 output statistics for 28,278 genes.
- Annotated DEG list (`TOTAL_DEG.csv`) containing 521 genes.

## 3. Which samples exist?
The historical/public project sample universe contains 23 runs (12 Tumor from PRJNA521318, 11 Control from PRJNA494155). 

## 4. Which samples were actually analyzed?
Exactly 21 samples were analyzed. 
- **11 Tumor**: SRR8561802 through SRR8561812
- **10 Control**: SRR7946355 through SRR7946364
SRR8561801 and SRR7946354 were not analyzed. The thesis explicitly states 11 tumor and 10 control samples were selected, though the specific reason for excluding these two runs remains unknown.

## 5. What computational steps can be demonstrated?
- **Demonstrated by provenance**: Transcript quantification (StringTie), read count merging (Galaxy Column Join), Differential Expression (DESeq2), and DEG filtering.
- **Demonstrated by report only**: Quality check (FastQC), Trimming (Trim-Galore), Alignment (HISAT2), Functional Enrichment (ShinyGO), PPI Network (STRING/Cytoscape/MCODE), and GSEA. 

## 6. What software and versions were used?
- **DESeq2**: Version 1.22.1 (R version 3.5.1)
- **ShinyGO**: v0.66
- **Cytoscape**: Version 3.6.1 (with MCODE plug-in)
- **GSEA**: Version 4.0.3

## 7. What parameters can be recovered?
- **Reference Genome**: hg38
- **DEG Filtering**: `padj <= 1e-6` AND (`log2FC >= 5` OR `log2FC <= -10`) AND `Gene.type == 'protein_coding'`.
- **PPI Network**: STRING combined score $\ge 0.7$.
- **MCODE**: Degree cutoff = 2, Node score cutoff = 0.2, k-core = 2, Max depth = 100.
- **GSEA**: 1000 gene set permutations against MSigDB.

## 8. What outputs exist?
- DESeq2 raw and normalized count matrices (rlog, VST).
- Volcano plots, MA plots, PCA plots, Heatmaps.
- `TOTAL_DEG.csv` (The filtered list of DEGs).

## 9. What historical results can be reproduced?
The core differential expression list can be exactly reproduced. Applying the recovered thresholds to the full DESeq2 result yields exactly the 521 genes found in `TOTAL_DEG.csv`. 

## 10. What contradictions remain?
- **Gene Count**: Thesis reports 523 DEGs (314 up, 209 down), but the recovered file has 521 DEGs (313 up, 208 down). This remains an unresolved discrepancy.

## 11. What information is permanently missing?
- Raw intermediate files for the upstream steps (FASTQ, BAM, FastQC reports).
- Scripts or session files for downstream tools (Cytoscape, GSEA).
- Direct documentation on why SRR8561801 and SRR7946354 were the specific samples excluded from the initial cohort.

## 12. Which parts can be modernized reliably?
The entire pipeline can be modernized reliably. The exact sample cohort is known, the differential expression thresholds are mathematically proven, and the downstream analysis goals (PPI, MCODE, GSEA) are well-defined.
