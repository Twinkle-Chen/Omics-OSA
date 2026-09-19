==============================================================================
Methylation regulation - DNA methylation based SMR
==============================================================================

1. What this folder is about

This folder holds the methylation layer of the study. DNA
methylation is used as the exposure of an SMR analysis in order to describe how
the prioritized signals are regulated, and three questions are addressed with the
SMR 1.3.1 executable:

   mQTL against sleep apnea        Does a methylation probe share its genetic
                                   signal with obstructive sleep apnea?
   mQTL against gene expression    Does the methylation probe act through the
                                   expression of the prioritized gene
                                   (mQTL-eQTL)?
   mQTL against protein level      Does the methylation probe act through the
                                   level of the prioritized protein
                                   (mQTL-pQTL)?

Both the blood and the brain methylation data are analysed. The shell scripts run
SMR with the 1000 Genomes European reference panel, and the R scripts collect the
per-chromosome output, apply the FDR correction, filter on the HEIDI p value
(p_HEIDI > 0.01) and export the significant methylation probes.

2. The merged code document

MethylationRegulation_Code_Merged.Rmd merges the 6 source files of this folder
into a single R Markdown document. Every source file became one code chunk: the
file name is given in the section heading, in the chunk title and in the first
comment line of the chunk, and the original chunk titles ("Read the blood DNAm SMR
results", ...) are kept as comments. The three shell scripts are kept in their own
bash chunks, and all chunks are set to eval = FALSE so that the document can be
knitted without the study data.

3. The code blocks and what they do

   SMRmQTL_SleepApnea_code.txt   (shell)
      Runs SMR of the blood mQTL data (one besd file per chromosome, listed in
      mqtlchr_EUR.txt) and of the brain methylation data (Brain-mMeta) against the
      FinnGen R12 sleep apnea GWAS, with --maf 0.01 and ten threads.

   SMRmQTL_SleepApnea_Rcode.Rmd
      Chunk "Read the blood DNAm SMR results": loops over the per-chromosome .smr
      files, appends them into one table and marks the significant probes in the
      mqtlchr list, applies the FDR correction, adds the beta and OR confidence
      intervals, writes ALL_Blood_SleepApnea.xlsx and Sig_Blood_SleepApnea.xlsx and
      exports the significant probes to Sig_Blood_SleepApnea_DNAm.txt.
      Chunk "Read the brain DNAm SMR results": the same for the brain methylation
      data (Sig_Brain_SleepApnea_DNAm.txt).

   SMRmQTLeQTL_code.txt   (shell)
      Runs SMR of the blood and the brain methylation data against the matching
      whole blood and brain eQTL besd files, restricted to the significant
      methylation probes of the previous step (--extract-exposure-probe) and to
      the significant gene probes (--extract-outcome-probe).

   SMRmQTLeQTL_Rcode.Rmd
      Chunk "Read the blood DNAm-gene SMR results" and chunk "Read the brain
      DNAm-gene SMR results": collect the per-chromosome mQTL-eQTL output, apply
      the FDR correction, add the beta and OR confidence intervals and keep the
      pairs with FDR < 0.05 and p_HEIDI > 0.01.

   SMRmQTLpQTL_code.txt   (shell)
      Runs SMR of the blood and the brain methylation data against the plasma and
      the brain protein pQTL besd files (the PWAS significant proteins), again
      restricted to the significant methylation probes and to the significant
      protein probes.

   SMRmQTLpQTL_Rcode.Rmd
      Chunk "Read the blood DNAm-protein SMR results" and chunk "Read the brain
      DNAm-protein SMR results": collect the mQTL-pQTL output, apply the FDR
      correction, add the beta and OR confidence intervals and keep the FDR < 0.05
      pairs with p_HEIDI > 0.01.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In MethylationRegulation_Code_Merged.Rmd those paths are rewritten
to short relative paths below the placeholder root Path/. Point that root at your
own data tree before running the code.

5. Software used in this folder

   - SMR 1.3.1 (smr-1.3.1-win.exe)
   - plink with the 1000 Genomes European reference panel (bfile)
   - R 4.3.1 with openxlsx, readxl, dplyr, readr and data.table

6. How to run

   1. Run SMRmQTL_SleepApnea first: it produces the significant methylation probes
      that the two later steps use as the exposure filter.
   2. Then run SMRmQTLeQTL and SMRmQTLpQTL, which also need the significant gene
      and protein probe lists of the SMR and colocalization folders.
   3. Put the mQTL, eQTL and pQTL besd files and the GWAS summary statistics below
      Path/datasource/ and the LD reference panel below Path/g1000/.
