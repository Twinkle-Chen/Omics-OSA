# Omics-OSA
This folder collects the analysis code of a study of potential biomarkers and drug targets for 
obstructive sleep apnea (OSA). The study screens the blood and brain molecular layers for
causal effects on OSA and on the five continuous traits that overnight oximetry
provides (apnea-hypopnea index, average oxygen desaturation, average oxygen
saturation, minimum oxygen saturation and the percentage of sleep time with an
oxygen saturation below 90%), and it then validates the resulting candidate
targets in independent samples.

The three molecular layers that are screened are:

   gene expression (transcriptome)   GTEx v8 whole blood and brain frontal cortex
                                     BA9, and the eQTLGen and BrainMeta resources
   protein abundance (proteome)      plasma proteins (European reference panel,
                                     ARIC, Iceland and Banner Health) and brain
                                     proteins (ROSMAP)
   DNA methylation (methylome)       blood and brain mQTL data

For every layer the analysis follows the same logic: a genome-wide association
test of the molecular trait (TWAS or PWAS), a summary-data-based Mendelian
randomization (SMR) test against the OSA GWAS, a colocalization analysis, and an
external validation of the prioritized targets in independent molecular QTL
resources. Two additional sections repeat the key analyses with body mass index
(BMI) taken into account and describe the functional enrichment and the
methylation regulation of the prioritized signals.

# The folders and what they contain

The folders are numbered by the order in which the analyses are run.

   TWAS/                     Transcriptome-wide association analysis of blood and
                             brain gene expression against OSA and the five
                             overnight oximetry traits (FUSION, GTEx v8 weights).

   PWAS/                     Proteome-wide association analysis of plasma and
                             brain protein levels against the same traits (FUSION
                             based PWAS weights), including the conversion of the
                             significant proteins into SMR input.

   SMR/                      Summary-data-based Mendelian randomization (SMR
                             1.3.1) of the eQTL (gene) and pQTL (protein) data
                             with the trait GWAS, with the HEIDI test.

   Colocalization/           Colocalization (coloc.abf) of the cis-regions of the
                             prioritized genes and proteins with the OSA GWAS, in
                             100 kb, 250 kb and 500 kb windows.

   ExternalValidation/       Two-sample Mendelian randomization of the prioritized
                             targets in independent molecular QTL resources
                             (eQTLGen, BrainMeta, Banner Health, Iceland) and in
                             independent OSA phenotypes (FinnGen R12 and OSA MVP),
                             with radial MR and MR-PRESSO sensitivity analyses.

   BMIAdjustment/            Multivariable MR of BMI together with the prioritized
                             targets, BMI-to-trait MR, and SMR of the targets
                             against a BMI GWAS to test whether the signal is
                             driven by the well known BMI-OSA relationship.

   Enrichment/               Gene ontology enrichment of the prioritized blood and
                             brain genes and the corresponding bubble plots.

   MethylationRegulation/    SMR analyses that describe the methylation layer of
                             the prioritized signals (mQTL against OSA, mQTL
                             against gene expression and mQTL against protein).

# The merged code documents

Every folder contains one merged R Markdown document that collects all analysis
scripts of that folder:

   TWAS/TWAS_Code_Merged.Rmd
   PWAS/PWAS_Code_Merged.Rmd
   SMR/SMR_Code_Merged.Rmd
   Colocalization/Colocalization_Code_Merged.Rmd
   ExternalValidation/ExternalValidation_Code_Merged.Rmd
   BMIAdjustment/BMIAdjustment_Code_Merged.Rmd
   Enrichment/Enrichment_Code_Merged.Rmd
   MethylationRegulation/MethylationRegulation_Code_Merged.Rmd

In total 89 source files (60 R Markdown scripts and 29 shell scripts) are merged.
Each source file became one code chunk; the name of the source file is given in
the section heading, in the chunk title and in the first comment line of the
chunk, and the original chunk titles are kept as comments inside the chunk. Shell
scripts are kept in their own bash chunks. The Chinese comments of the original
scripts were translated into English and the date of the original document header
is not part of the merged files. All chunks are set to eval = FALSE, so a merged
document can be knitted without the study data; set eval = TRUE, or run a single
chunk, after the paths have been pointed at your own copy of the data. Each
folder also has its own README.txt that explains the folder and lists the purpose
of every code chunk.

# Path convention used in the merged documents

The original scripts refer to the analysis tree of the workstation on which they
were written. In the merged documents all of those paths are rewritten to short
relative paths below the placeholder root Path/. Point Path/ at your own data
tree before running the code.

# Software used

   - FUSION (fusion_twas-master, FUSION.assoc_test.R) with plink and the 1000
     Genomes European LD reference panel
   - SMR 1.3.1 (smr-1.3.1-win.exe)
   - R 4.3.1 with, among others, TwoSampleMR, MendelianRandomization, MRPRESSO,
     RadialMR, mr.raps, coloc, locuscomparer, clusterProfiler, enrichplot,
     org.Hs.eg.db, openxlsx, readxl, data.table, dplyr and ggplot2
   - plink 1.90 (through plinkbinr) for the LD clumping of the instruments

# How to run the pipeline

   1. Rebuild the data tree below Path/ (see the table in section 4) and adjust
      the R interpreter and the SMR executable paths if your installation differs.
   2. Work through the folders in the order TWAS, PWAS, SMR, Colocalization,
      ExternalValidation, BMIAdjustment, Enrichment and MethylationRegulation.
      Each step consumes the exported workbooks of the previous one: TWAS and PWAS
      produce the input lists of SMR, SMR produces the significant probes, and
      those probes are used again by the colocalization, external validation, BMI
      and methylation sections.
   3. Inside a merged document, run the shell chunks first (they produce the
      per-chromosome or per-probe files) and then the R chunks that collect and
      filter the results.

# Notes

   - Every merged document ends with the analysis code as it was run; the file
     paths, the Chinese comments and the document dates are the only content that
     was adapted. The statistical code itself is unchanged.
   - The Excel tables that the scripts read (for example the enrichment result
     tables) are intermediate outputs of the same pipeline; run the preceding
     chunk if they are not available.
