==============================================================================
SMR - summary-data-based Mendelian randomization
==============================================================================

1. What this folder is about

This folder holds the SMR and HEIDI analyses of the study.
SMR tests whether the effect of a molecular trait (gene expression or protein
abundance) on obstructive sleep apnea and on the five overnight oximetry traits
can be explained by a shared cis-acting genetic variant, and the HEIDI test
separates that shared signal from linkage. The SMR executable 1.3.1 is used for
all analyses.

Two layers are analysed for every trait:

   gene (eQTL)      - blood (GTEx v8 whole blood) and brain (frontal cortex BA9)
                      eQTL besd files, restricted to the FDR < 0.05 TWAS genes
   protein (pQTL)   - plasma and brain protein pQTL besd files that are built from
                      the flists of the significant PWAS proteins

The shell scripts run the SMR tests and extract the probe-level data of the
significant genes; the R scripts collect the per-chromosome output, apply the FDR
correction, filter on the HEIDI p value (p_HEIDI > 0.01) and export the significant
probes, which are the input of the colocalization folder.

2. The merged code document

SMR_Code_Merged.Rmd merges the 24 source files of this folder into a single R
Markdown document. Every source file became one code chunk: the file name is given
in the section heading, in the chunk title and in the first comment line of the
chunk, and the original chunk titles ("Read the blood mRNA SMR results", ...) are
kept as comments. The twelve shell scripts are kept in their own bash chunks.
All chunks are set to eval = FALSE so that the document can be knitted without the
study data.

3. The code blocks and what they do

   SMReQTL_<trait>_code.txt   (shell, one file per trait)
      Runs the SMR test of the blood and of the brain eQTL besd file against the
      trait GWAS (--bfile g1000/EUR, --gwas-summary, --beqtl-summary,
      --maf 0.01) and then queries the besd file probe by probe (--query 1
      --probe) for every significant gene of the Sig_*_gene.txt list, which is the
      eQTL data that the colocalization and the external validation sections use.
      Traits: SleepApnea, AHI, AvgDesat, AvgSpo2, MinSpo2 and Per90.

   SMReQTL_<trait>_Rcode.Rmd   (one file per trait)
      Chunk "Read the blood mRNA SMR results": reads the Blood_<trait>.smr output,
      keeps the probes that are significant in TWAS, applies the FDR correction,
      filters on p_HEIDI > 0.01, writes ALL_Blood_<trait>.xlsx and
      Sig_Blood_<trait>.xlsx and exports the significant probe list.
      Chunk "Read the brain mRNA SMR results": the same for the brain layer.

   SMRpQTL_<trait>_code.txt   (shell, one file per trait)
      Builds the plasma and the brain protein besd files from the pQTL flists
      (--make-besd) and runs the SMR test of those besd files against the trait
      GWAS for the plasma proteins, the brain proteins and the ARIC proteins.
      Traits: SleepApnea, AHI, AvgDesat, AvgSpo2, MinSpo2 and Per90.

   SMRpQTL_<trait>_Rcode.Rmd   (one file per trait)
      Chunk "Read the plasma protein SMR results" and chunk "Read the brain protein
      SMR results": read the .smr output, apply the FDR correction, filter on
      p_HEIDI > 0.01, write the ALL_ and Sig_ workbooks for the plasma and the
      brain proteins and export the significant probe lists.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In SMR_Code_Merged.Rmd those paths are rewritten to short relative
paths below the placeholder root Path/. Point that root at your own data tree
before running the code.

5. Software used in this folder

   - SMR 1.3.1 (smr-1.3.1-win.exe)
   - plink 1.90 with the 1000 Genomes European reference panel (bfile)
   - R 4.3.1 with openxlsx, readxl, dplyr, readr and data.table

6. How to run

   1. Put the eQTL and pQTL besd files, the GWAS summary statistics in SMR format,
      the probe lists and the 1000 Genomes reference panel below Path/datasource/
      and Path/g1000/.
   2. Run the shell chunks first (the SMR tests and the probe-level extraction),
      then the matching R chunks that collect and filter the results.
   3. Adjust the SMR executable path (Path/SMR/smr-1.3.1-win-x86_64/
      smr-1.3.1-win.exe) if your installation differs.
