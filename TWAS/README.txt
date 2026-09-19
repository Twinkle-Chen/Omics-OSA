==============================================================================
TWAS - transcriptome-wide association analysis
==============================================================================

1. What this folder is about

This folder holds the transcriptome-wide association (TWAS) step of the study. 
Genetically predicted gene expression is tested for an
association with obstructive sleep apnea and with the five continuous traits that
are derived from overnight oximetry, namely the apnea-hypopnea index (AHI), the
average oxygen desaturation (AvgDesat), the average oxygen saturation (AvgSpo2),
the minimum oxygen saturation (MinSpo2) and the percentage of sleep time with an
oxygen saturation below 90 % (Per90).

Two expression panels are used for every trait:

   blood  - GTEx v8 whole blood (GTExv8.EUR.Whole_Blood.pos)
   brain  - GTEx v8 frontal cortex BA9 (GTExv8.EUR.Brain_Frontal_Cortex_BA9.pos)

Gene expression is imputed with the FUSION elastic net weights and the association
test is run with FUSION.assoc_test.R, chromosome by chromosome. The R scripts then
merge the per-chromosome output files (including the separate MHC file of
chromosome 6), apply a Benjamini-Hochberg FDR correction to the TWAS p value and
keep the genes with FDR < 0.05. The significant genes are the input of the SMR and
colocalization sections.

2. The merged code document

TWAS_Code_Merged.Rmd merges the 12 source files of this folder into a single R
Markdown document. Every source file became one code chunk: the file name is given
in the section heading, in the chunk title and in the first comment line of the
chunk. The six shell scripts are kept in their own bash chunks. All chunks are set
to eval = FALSE so that the document can be knitted without the study data; set
eval = TRUE, or run a single chunk, after the paths point at your own copy of the
data.

3. The code blocks and what they do

   TWAS_SleepApnea_code.txt   (shell)
      Runs FUSION.assoc_test.R for the FinnGen R12 sleep apnea GWAS summary
      statistics for the 22 autosomes, first with the GTEx v8 whole blood weights
      and then with the GTEx v8 brain frontal cortex BA9 weights. The
      per-chromosome output is written to the GTEx_TWAS_by_chr and
      GTExBrain_TWAS_by_chr folders.

   TWAS_SleepApnea_Rcode.Rmd
      Chunk "Merge blood TWAS results and export": merges the per-chromosome blood
      output, corrects the p values (FDR) and writes the complete table
      (BloodGene_SleepApnea_TWAS.xlsx) together with the FDR < 0.05 subset.
      Chunk "Merge brain TWAS results and export": the same for the brain panel.

   TWAS_AHI_code.txt / TWAS_AHI_Rcode.Rmd
      The same pair of analyses for the apnea-hypopnea index.

   TWAS_AvgDesat_code.txt / TWAS_AvgDesat_Rcode.Rmd
      The same pair of analyses for the average oxygen desaturation.

   TWAS_AvgSpo2_code.txt / TWAS_AvgSpo2_Rcode.Rmd
      The same pair of analyses for the average oxygen saturation.

   TWAS_MinSpo2_code.txt / TWAS_MinSpo2_Rcode.Rmd
      The same pair of analyses for the minimum oxygen saturation.

   TWAS_Per90_code.txt / TWAS_Per90_Rcode.Rmd
      The same pair of analyses for the percentage of sleep time with an oxygen
      saturation below 90 %.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In TWAS_Code_Merged.Rmd those paths are rewritten to short relative
paths below the placeholder root Path/. Point that root at your own data tree
before running the code.

5. Software used in this folder

   - FUSION (fusion_twas-master, FUSION.assoc_test.R) with plink and the LD
     reference panel 1000G.EUR
   - R 4.3.1 with openxlsx, readxl, dplyr, readr, GenomicRanges, rtracklayer and
     data.table

6. How to run

   1. Put the input files below Path/datasource/ (the GWAS summary statistics in
      FUSION format and the TWAS weight files) and the FUSION scripts below
      Path/TWAS/Official_FUSION/.
   2. Adjust the R interpreter path (Path/R-4.3.1/bin/Rscript.exe) to your own
      installation.
   3. Run the shell chunks first, chromosome by chromosome, and then the matching
      R chunks that merge the per-chromosome output.
