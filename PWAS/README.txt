==============================================================================
PWAS - proteome-wide association analysis
==============================================================================

1. What this folder is about

This folder holds the proteome-wide association (PWAS) step of the study. 
Genetically predicted protein abundance is tested for an
association with obstructive sleep apnea and with the five continuous traits of
the overnight oximetry (apnea-hypopnea index, average oxygen desaturation, average
oxygen saturation, minimum oxygen saturation and the percentage of sleep time
with an oxygen saturation below 90 %).

Two protein panels are used for every trait:

   plasma proteins  - European plasma protein reference weights
                      (PWAS_EA/Plasma_Protein_EA_hg19.pos, output in PWAS_by_chr)
   brain proteins   - ROSMAP brain protein weights
                      (ROSMAP/train_weights.pos, output in ROSMAP_PWAS_by_chr)

The association test is run with PWAS.assoc_test.R chromosome by chromosome. The R
scripts merge the per-chromosome output, apply a Benjamini-Hochberg FDR correction
and export the complete and the significant (FDR < 0.05) result tables. For sleep
apnea, the apnea-hypopnea index and the average oxygen desaturation the pQTL
summary statistics of the significant proteins are additionally converted into the
SMR input format (lifted over to hg19, effect allele selected, probe list built),
because those proteins are the input of the SMR folder.

2. The merged code document

PWAS_Code_Merged.Rmd merges the 12 source files of this folder into a single R
Markdown document. Every source file became one code chunk: the file name is given
in the section heading, in the chunk title and in the first comment line of the
chunk, and the original chunk titles ("Merge plasma PWAS results and export",
"Convert the pQTL data of significant plasma proteins into SMR format", ...) are
kept as comments. The six shell scripts are kept in their own bash chunks. All
chunks are set to eval = FALSE so that the document can be knitted without the
study data.

3. The code blocks and what they do

   PWAS_SleepApnea_code.txt   (shell)
      Runs PWAS.assoc_test.R for the FinnGen R12 sleep apnea GWAS, chromosome by
      chromosome, first with the plasma protein weights and then with the ROSMAP
      brain protein weights.

   PWAS_SleepApnea_Rcode.Rmd
      Chunk "Merge plasma PWAS results and export": merges the per-chromosome
      plasma output, applies the FDR correction and writes
      PlasmaPro_SleepApnea_PWAS.xlsx.
      Chunk "Convert the pQTL data of significant plasma proteins into SMR
      format": selects the significant plasma proteins, removes duplicates and
      converts their pQTL summary statistics (effect allele, hg38 to hg19
      liftover, probe list) into the SMR format.
      Chunk "Merge brain PWAS results and export" and chunk "Convert the pQTL data
      of significant brain proteins into SMR format": the same two steps for the
      ROSMAP brain protein panel.

   PWAS_AHI_code.txt / PWAS_AHI_Rcode.Rmd
      The same pair of analyses for the apnea-hypopnea index, including the SMR
      format conversion for the plasma and the brain proteins.

   PWAS_AvgDesat_code.txt / PWAS_AvgDesat_Rcode.Rmd
      The same pair of analyses for the average oxygen desaturation, including the
      SMR format conversion for the plasma and the brain proteins.

   PWAS_AvgSpo2_code.txt / PWAS_AvgSpo2_Rcode.Rmd
      PWAS of the plasma and brain proteins for the average oxygen saturation and
      the export of the significant proteins (no SMR format conversion).

   PWAS_MinSpo2_code.txt / PWAS_MinSpo2_Rcode.Rmd
      PWAS of the plasma and brain proteins for the minimum oxygen saturation and
      the export of the significant proteins (no SMR format conversion).

   PWAS_Per90_code.txt / PWAS_Per90_Rcode.Rmd
      PWAS of the plasma and brain proteins for the percentage of sleep time with
      an oxygen saturation below 90 % and the export of the significant proteins
      (no SMR format conversion).

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In PWAS_Code_Merged.Rmd those paths are rewritten to short relative
paths below the placeholder root Path/. Point that root at your own data tree
before running the code.

5. Software used in this folder

   - FUSION style PWAS (PWAS.assoc_test.R) with plink and the European LD
     reference panel 1000G.EUR
   - R 4.3.1 with openxlsx, readxl, dplyr, readr, GenomicRanges, rtracklayer and
     data.table

6. How to run

   1. Put the GWAS summary statistics, the PWAS weights, the LD reference panel
      and the pQTL summary statistics below Path/datasource/, and the PWAS script
      and the liftover chain file below Path/.
   2. Run the shell chunks first to create the per-chromosome output, then the R
      chunks that merge the results and build the SMR input.
