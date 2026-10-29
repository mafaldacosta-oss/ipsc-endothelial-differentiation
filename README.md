# iPSC-to-Endothelial Cell Differentiation

R Markdown scripts supporting the transcriptomic analyses presented in the MSc thesis:

Strategies for iPSC Differentiation into Endothelial Cells: A Literature and Transcriptomic Analysis of 2D and 3D Approaches Toward Organoid Vascularization

Instituto Superior Técnico, 2026.

## Overview

The analyses investigate changes in gene expression and biological programmes along iPSC-to-endothelial cell differentiation. They combine differential expression, gene set enrichment, cross-dataset comparisons, curated molecular signatures, protein interaction networks and transcription factor prioritisation.

The main differentiation trajectory uses GSE214306 as the backbone dataset, with iPSC, early mesoderm progenitor cell (eMPC) and endothelial cell (EC) stages. GSE271101 provides the principal cross-dataset comparison. GSE263058 and GSE271456/GSE280017 support additional assessments of canonical marker expression.

## Analysis scripts

- Backbone_RAW_treatment.Rmd: Backbone count processing, identifier mapping, filtering, normalisation, PCA, marker visualisation and differential expression.

- Validation_GSE271101.Rmd: GSE271101 count processing, marker assessment and differential expression.
  
- Validation_GSE263058.Rmd: GSE263058 FPKM processing and marker assessment.

- Validation_GSE271456_GSE280017.Rmd: Combined RSEM-derived expression processing and marker assessment.

- Validation_Summary.Rmd: Comparative marker summaries, heatmaps and expression profiles.

- DEG&GSEA_apriori.Rmd: DEG analysis, GO BP and Hallmark GSEA, common-universe analyses, replicated DEGs, leading-edge extraction, ORA and stable-gene analyses.

- MolecularSignatures.Rmd: Gene trajectories, module scores and representative core-gene visualisations.

- STRING.Rmd: Module-level protein interaction networks and network statistics.

- TFTrack.Rmd: Transcription factor evidence integration, inferred regulon activity and sensitivity analyses.

- IntegratedTimeline.Rmd: Integration of regulatory, signalling and module-level results.

- TF_manipulability_screening.Rmd: Preparation of a transcription factor intervention-screening table for manual literature curation.

## Execution order

1. Run the four dataset-processing scripts: Backbone_RAW_treatment.Rmd, Validation_GSE271101.Rmd, Validation_GSE263058.Rmd and Validation_GSE271456_GSE280017.Rmd. These process independent inputs.

2. Run Validation_Summary.Rmd after the four dataset scripts have produced their respective mat_z.csv files within results/.

3. Run DEG&GSEA_apriori.Rmd after the backbone and GSE271101 scripts. Sections 1–6.2 generate the enrichment results used for pathway curation.

4. Supply GSEA_GO_BP_curated_apriori.xlsx before continuing from Section 6.3 onward. This workbook contains manual pathway-inclusion decisions and biological-category assignments. These decisions are not automatically regenerated.

5. Run MolecularSignatures.Rmd, STRING.Rmd and TFTrack.Rmd after their required DEG/GSEA outputs are available. These three scripts do not require one another's outputs.

6. Run IntegratedTimeline.Rmd after the DEG/GSEA, molecular-signature and TF analyses.

7. Run TF_manipulability_screening.Rmd after IntegratedTimeline.Rmd has produced TF_selection_table.xlsx.

Validation_Summary.Rmd is not a prerequisite for the DEG/GSEA analyses.

## Input data and supporting files

Expression datasets are available through NCBI GEO under the accession numbers GSE214306, GSE271101, GSE263058, GSE271456 and GSE280017.

The scripts expect the following inputs:

- Backbone featureCounts tables: Backbone_Raw/
- GSE271101 count matrix: GSE271101_raw_counts.csv.gz
- GSE263058 FPKM matrix: GSE263058_FPKM.all.txt.gz
- GSE280017 RSEM files: GSE280017_RAW/
- GSE271456 RSEM files: GSE271456_RAW/
- Manually curated GO BP workbook: GSEA_GO_BP_curated_apriori.xlsx
- Backbone Ensembl-to-HGNC mapping: ensembl_hgnc_map.rds
- Additional-dataset Ensembl-to-HGNC mapping: ensembl_map_gse271456_gse280017.rds
- AnimalTFDB4 catalogue for TF cross-checks: all_tf_list.txt

The backbone script expects nine featureCounts tables:

- iPSC_K15.tabular
- iPSC_K37.tabular
- iPSC_K53.tabular
- eMPC_K15.tabular
- eMPC_K37.tabular
- eMPC_K53.tabular
- EC_K15.tabular
- EC_K37.tabular
- EC_K53.tabular

Upstream sequencing-read processing, alignment and counting are described in the thesis and are not implemented by these R scripts.

Raw expression inputs and regenerable intermediate results are excluded from version control. The curated GO BP workbook is an essential input for downstream analyses: no script reconstructs its manual inclusion decisions or biological-category assignments.

The mapping tables were generated from Ensembl release 116, as documented in the thesis. They preserve the identifier mappings used in the analyses. If a mapping file is absent, the corresponding script attempts to query Ensembl again; a new query may return different mappings as the database changes.

## Versions used in the thesis

- R 4.5.3
- Ensembl release 116 for identifier mapping
- msigdbr v26.1.0, providing MSigDB 2026.1.Hs

These are the versions documented for the thesis analyses. Re-running with newer releases may change identifier mappings, gene set contents and enrichment results. In particular, msigdbr v26.1.1 should not be assumed to reproduce results generated with v26.1.0.

Other dependencies are declared within the individual scripts. Principal packages include limma, edgeR, dplyr, tidyr, ggplot2, ComplexHeatmap, circlize, readxl, writexl, fgsea, clusterProfiler, org.Hs.eg.db, STRINGdb, igraph, decoupleR and OmnipathR.

## Execution notes and limitations

The scripts retain working-directory settings and paths from the original analysis computer. Adapt these paths before execution, including paths to the mapping files and curated workbook.

Intermediate objects and tables must be generated in the documented order and retained locally, even when excluded from version control.

In the original IntegratedTimeline.Rmd, the leading-edge object le_all is used without being loaded or constructed within that notebook. Execution therefore requires that object to be available from the preceding analyses, or an explicit export/import to be added before standalone execution. The existing write.csv call exports the object; it does not load it.

TFTrack.Rmd optionally compares results with TF_manipulability_screening_2026-09-20.xlsx when that workbook exists. This comparison is skipped otherwise.

TF_manipulability_screening.Rmd exports a dated screening-table skeleton containing preliminary literature entries. It does not automatically reconstruct the completed literature screening reported in the thesis.

The integrated timeline script prepares analytical content and a plot. The final figure layout in the thesis also involved manual layout work.

Legacy object names containing "sustained" refer to the gene set described as "stable" in the thesis.

## Reproducibility status

This repository makes the analytical code available for inspection and reuse. A clean, end-to-end reproduction of the repository version has not been verified.

Exact agreement with the thesis requires the original inputs, curated decisions, identifier mappings and software/resource versions. No package lockfile is currently supplied.

## License

The code is distributed under the MIT License; see LICENSE. Third-party datasets and resources remain subject to their respective terms.
