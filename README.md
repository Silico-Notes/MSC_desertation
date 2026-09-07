# Identification of Genomic Biomarkers in Cervical Carcinomas Using Bioinformatics Approaches

This repository documents and preserves an MSc computational biology project investigating transcriptional differences between human cervical cancer samples and healthy controls using public NCBI SRA RNA-seq data. The repository is organized as a professional historical research portfolio, explicitly separating the documented thesis claims from the surviving computational evidence.

## Project Overview

| Evidence category | Meaning in this repository |
| --- | --- |
| Recovered | Directly supported by surviving computational files or outputs |
| Thesis-reported | Explicitly stated in the original MSc thesis |
| Reconstructed | Derived by comparing surviving evidence with thesis claims |
| Unresolved | Contradictions or uncertainties that cannot be responsibly resolved |

This classification prevents historical claims from being presented as independently reproduced results.

Cervical cancer is a leading gynecological malignancy strongly associated with persistent infection by high-risk human papillomaviruses (HPV), which drives extensive molecular and transcriptional alterations. This project computationally investigated both squamous cell carcinoma (SCC) and adenocarcinoma subtypes to answer a central research question: *Which genes and biological pathways show significant transcriptional differences between cervical tumor tissue and healthy control samples, and which of these genes may represent candidate biomarkers or central components of disease-associated molecular networks?*

## Aims and Objectives

The original thesis defined three primary aims:
1. Identification of differentially expressed genes from SRA tumor/control datasets.
2. Protein-protein interaction analysis of the identified genes.
3. Functional enrichment analysis of the dysregulated gene sets.

## Dataset and Study Design

The project utilized publicly available RNA-seq data obtained from the NCBI Sequence Read Archive (SRA).

* **Control Accession**: PRJNA494155
* **Tumor Accession**: The thesis *Methods* section identifies PRJNA521318, whereas the *Results* section references PRJNA636799. This provenance discrepancy is explicitly preserved.
* **Tumor Cohort**: 11 total samples (described as 8 SCC + 3 adenocarcinoma).
* **Control Cohort**: 10 selected healthy samples.

**Sample Discrepancy Note**: The historical public universe spans 12 tumor accessions (`SRR8561801`–`SRR8561812`) and 11 control accessions (`SRR7946354`–`SRR7946364`). However, the recovered computational analysis matrix contains exactly 21 samples (`SRR8561802`–`SRR8561812` for tumors, `SRR7946355`–`SRR7946364` for controls). `SRR8561801` and `SRR7946354` remain unresolved discrepancies and are not presented here as intentionally excluded.

## Historical Computational Workflow

The original MSc analysis was a sequential computational pipeline:

`SRA → FastQC → Trim Galore → HISAT2 → StringTie → DESeq2 → Differential Expression → GO/KEGG enrichment → STRING PPI → Cytoscape → MCODE → Hub-gene analysis → GSEA`

*Note on Workflow Tooling*: While some historical diagrams referenced edgeR or Cuffdiff, the detailed methodology and surviving computational evidence support DESeq2 as the differential-expression engine represented in the recovered workflow.

## Quality Control and Quantification

The upstream sequence processing was largely executed via Galaxy (v21.05):
* **Quality Assessment**: FastQC evaluated the raw ~75 bp reads.
* **Trimming**: Trim Galore handled adapter and quality trimming.
* **Alignment**: HISAT2 performed splice-aware mapping against the `hg38` human reference genome.
* **Quantification**: StringTie (v2.1.1) was used for transcript assembly and quantification.
* **Differential Expression**: DESeq2 was used for statistical differential-expression analysis; surviving Galaxy evidence records the DESeq2 tool/version information as 2.11.40.6, with R 3.5.

## Differential Expression Results

The thesis reports 14,160 genes in its differential-expression result set (8,125 downregulated and 6,035 upregulated). To isolate the most significant biological candidates, a stringent historical filtering scheme was then applied:

* **Adjusted significance threshold**: padj ≤ 1e-6 (reported historically as FDR in parts of the thesis)
* **Fold-Change Filters**: log2 fold change ≤ -10 (downregulated) OR log2 fold change ≥ 5 (upregulated)

**Discrepancy Note**:
* **Thesis-reported result**: 523 DEGs (314 upregulated + 209 downregulated).
* **Recovered result**: 521 `protein_coding` genes.
* The difference of two genes remains an unresolved discrepancy. This repository embraces this distinction to demonstrate provenance-aware reproducibility rather than hiding the inconsistency.

Surviving visual outputs from this phase (PCA, MA plots, dispersion plots, heatmaps, FDR distributions) are preserved in the repository.

## Functional Enrichment Analysis

ShinyGO (v0.66) was used to execute Gene Ontology (GO) and KEGG pathway enrichment on the final DEGs.

### Gene Ontology (GO)
Across Biological Process, Cellular Component, and Molecular Function domains, historical GO themes included: biological regulation, metabolic process, cellular component organization, response to stimulus, nucleus, membrane-enclosed lumen, macromolecular complex, chromosome, membrane, protein binding, nucleic acid binding, hydrolase activity, and ion binding.

### KEGG Pathway Findings
Seventy pathways were deemed significant at P < 0.05, with 10 pathways highlighted for specific discussion. Major pathway themes included:
* Viral response & carcinogenesis (influenza A, tuberculosis, herpes simplex, Epstein-Barr virus, viral carcinogenesis).
* Cell cycle & reproduction (cell cycle, oocyte meiosis, progesterone-mediated oocyte maturation).
* Immune & signaling (chemokine signaling, RIG-I-like receptor, Toll-like receptor, IL-17, VEGF, AGE-RAGE).
* Invasion & cancer (focal adhesion, Rap1, Ras, PI3K-Akt, proteoglycans in cancer, pathways in cancer, endocrine resistance).

These enriched pathways suggest coordinated signals involving cell-cycle regulation, innate immune responses, and invasion mechanisms. They represent computational associations, not proof of HPV causality.

## PPI Network Analysis

Protein-protein interactions (PPI) were analyzed using STRING:
* **Combined Interaction Score**: ≥ 0.7
* **Expected Edges**: 282
* **PPI Enrichment p-value**: < 1.0e-16

The significant PPI enrichment indicates that the submitted proteins had more interactions than expected by chance under the STRING model, supporting the use of network topology for candidate prioritization.

## Cytoscape / MCODE Network Analysis

Cytoscape (v3.6.1) and the MCODE plugin were used to identify highly interconnected network modules.
* **Parameters**: Cutoff = 2, Node score cutoff = 0.2, K-core = 2, Maximum depth = 100.

**Hub-Gene Discrepancy Note**: The historical sources contain multiple levels of reporting: the abstract/conclusion reports 317 hub genes, the detailed Results section describes 100 recurring genes/candidates, and it also details additional smaller phase-specific candidate sets. Surviving evidence does not reconcile these counts, which represents an unresolved historical reporting issue.

## Key Historical Candidate Genes

The thesis identified numerous high-priority candidate genes occupying central network positions. Examples include:
* CDK1, CCNB1, FN1, ITGB1, ITGB6, MMP9, MMP10, FPR1, FPR2, GNG12, GNB3, GNB4, GNG13, PPBP, STAT1, GBP1, IFI44L, IFIT3, IFI1, IFIT5, IFI44, IFIT2, IFI6, IRF7, CXCL10, HGF, IGF1, KIT, CXCL12, CCND1, VEGFA, CXCL8, CXCL11, CXCL4, ITGA1, MAPK12, PIK3CA, FOS.

*Note: These are computational candidates, not experimentally validated clinical biomarkers.*

Historical network degree values emphasized the topological importance of specific genes and pathways:
* **Gene Degrees**: CDK1 (16), FN1 (12), ITGB1 (8).
* **Pathway Degrees**: MAPK signaling (33), PI3K-Akt (21), focal adhesion (15).

## GSEA

Gene Set Enrichment Analysis (GSEA v4.0.3) was historically utilized with MSigDB curated sets and 1,000 permutations. While part of the historical methodology, detailed GSEA result tables (NES values, FDR scores) are not currently recovered in the surviving project materials.

## Biological Interpretation

The historical analysis highlighted three broad biological themes:
1. **Cell-cycle/proliferation regulation**, particularly CDK1/CCNB1-associated processes.
2. **Inflammatory, innate immune, and viral-response signaling**.
3. **Adhesion, extracellular-matrix, and oncogenic signaling** associated with invasion and metastatic biology.

## Key Results at a Glance

| Metric | Value |
|--------|-------|
| Thesis-reported differential-expression result set | 14,160 |
| Thesis-reported final candidates | 523 |
| Recovered DEG Table | 521 |
| Upregulated DEGs | 314 |
| Downregulated DEGs | 209 |
| Significant KEGG Pathways | 70 |
| Highlighted Pathways | 10 |
| STRING Expected Edges | 282 |
| STRING PPI Enrichment | < 1e-16 |
| Top Network Proteins | CDK1, FN1, ITGB1 |
| Top Pathway-Network Nodes | MAPK, PI3K-Akt, Focal Adhesion |

## Repository Structure

* `README.md` — This file.
* `LICENSE` — Open-source license details.
* `docs/` — Documentation layer providing provenance and methodological context.
  * [HISTORICAL_WORKFLOW.md](docs/HISTORICAL_WORKFLOW.md) — Detailed description of the original computational workflow.
  * [FORENSIC_INVENTORY.md](docs/FORENSIC_INVENTORY.md) — Inventory of recovered project evidence.
  * [FORENSIC_SUMMARY.md](docs/FORENSIC_SUMMARY.md) — Summary of the computational recovery.
  * [RECONSTRUCTION_LOG.md](docs/RECONSTRUCTION_LOG.md) — Records what could and could not be reconstructed.
  * [FILE_MANIFEST.tsv](docs/FILE_MANIFEST.tsv) — Structured inventory of relevant project files.
  * [FUTURE_WORK.md](docs/FUTURE_WORK.md) — Proposed modernization and reproducibility work.
* `results/historical/` — Contains the surviving outputs from the original MSc analysis, including the recovered DEG table (`TOTAL_DEG.csv`) and PDF heatmaps/plots.

## Reproducibility and Provenance

An important principle of this repository is the strict separation of four categories of evidence:
1. **Directly recovered**: Facts supported by surviving files/output.
2. **Thesis-reported**: Claims documented in the MSc thesis.
3. **Reconstructed**: Values or thresholds derived by comparing surviving files and thesis claims.
4. **Unresolved**: Contradictions that cannot responsibly be resolved.

This distinction is a major feature of the portfolio, ensuring scientific traceability without retrospective manipulation.

## Limitations

Several limitations contextualize this historical portfolio:
* The retrospective and *in-silico* nature of the study lacks experimental wet-lab validation.
* Results are highly sensitive to sample sizes; interpretations could change with additional data.
* The stringent filtering criteria (padj ≤ 1e-6, |log2FC| ≥ 5/10) may have removed biologically relevant genes.
* HPV-specific interpretation was limited by the available study design and downstream enrichment results; although HPV-related biology was discussed in the thesis, a dedicated HPV pathway was not recovered among the reported enriched pathways.
* Discrepancies regarding accessions (PRJNA521318 vs PRJNA636799), samples (21 vs 23), and DEGs (521 vs 523) remain unresolved.
* Downstream analysis outputs (e.g., GSEA tables) were not fully recovered.

## Historical Work vs Future Work

**CURRENT REPOSITORY**: Preserves only the historical MSc work and surviving evidence.
**FUTURE WORK**: Proposed plans for modern reproducible reanalysis, workflow automation, updated annotation, modern statistical analysis, and expanded validation are separately documented in [FUTURE_WORK.md](docs/FUTURE_WORK.md).

## What This Project Demonstrates

This portfolio demonstrates practical experience with the end-to-end computational systems-biology workflow:
* Retrieving and interpreting public NCBI SRA data.
* RNA-seq preprocessing (quality control, read trimming).
* Splice-aware alignment and transcript quantification.
* Statistical differential expression and stringent historical candidate filtering.
* Functional enrichment via GO and KEGG interpretation.
* PPI analysis using STRING.
* Network-based candidate prioritization using Cytoscape and MCODE.
* Translation of high-dimensional transcriptomic data into testable biological hypotheses.
* Transparent provenance and reproducibility assessment.

## Portfolio Context

This repository represents an MSc-era computational biology project reorganized into a transparent portfolio format. It intentionally preserves historical results and clearly identifies unresolved provenance issues instead of retroactively modifying the original conclusions.

## Citation / References

The primary historical source for this repository is the original MSc thesis:
* **Dahale, Sameer. (2021).** *Identification of Genomic Biomarkers in Cervical Carcinomas Using Bioinformatics Approaches.* MSc Bioinformatics, Centre for Bioinformatics, Pondicherry University.

Critical tooling referenced in this work includes NCBI SRA, Galaxy, FastQC, Trim Galore, HISAT2, StringTie, DESeq2, ShinyGO, STRING, Cytoscape, MCODE, and GSEA/MSigDB.
