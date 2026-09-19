==============================================================================
External validation of the prioritized targets in independent samples
==============================================================================

1. What this folder is about

This folder holds the external validation step of the study. Every prioritized target is re-tested with two-sample Mendelian
randomization in molecular QTL resources that were not used in the screening, and
against a second sleep apnea phenotype:

   molecular resources   eQTLGen  (blood eQTL, large blood meta-analysis)
                         BrainMeta (brain eQTL meta-analysis)
                         Banner Health BBDP (plasma proteomics)
                         Iceland (plasma proteomics, deCODE)
   sleep apnea phenotypes FinnGen R12 sleep apnea
                         OSA MVP  (Million Veteran Program sleep apnea)

For every target-variant set the scripts follow the same three steps: define the
instruments (independent variants with p < 5e-8, LD clumped against the 1000
Genomes European reference panel), harmonise the exposure (QTL) and the outcome
(sleep apnea) datasets and save the harmonised dataset, and then run the MR
analysis (inverse variance weighted with the other TwoSampleMR methods). The
RadialMR scripts repeat the same instruments with radial MR to detect and remove
outliers, and the MR-PRESSO scripts summarise the MR-PRESSO global test, the
outlier test and the distortion test for all of the harmonised datasets.

2. The merged code document

ExternalValidation_Code_Merged.Rmd merges the 15 source files of this folder into
a single R Markdown document. Every source file became one code chunk: the file
name is given in the section heading, in the chunk title and in the first comment
line of the chunk, and the original chunk titles ("select the protein summary",
"setting", "harmonise", "MR") are kept as comments. All chunks are set to
eval = FALSE so that the document can be knitted without the study data.

3. The code blocks and what they do

   Banner_protein_sleepapnea_MR.Rmd
      Validates the Banner Health plasma proteins (TMEM106B, IFIT2, BCL2L13) with
      the FinnGen R12 sleep apnea phenotype. Chunk "select the protein summary"
      extracts the protein summary statistics, chunk "setting" defines the targets
      and loads the GWAS, chunk "harmonise" builds and saves the harmonised
      datasets (Banner/harmonise_data/) and chunk "MR" runs the MR analysis.

   Banner_protein_OSAMVP_MR.Rmd
      The same analysis with the OSA MVP phenotype as the outcome.

   BrainMeta_gene_sleepapnea_MR.Rmd / BrainMeta_gene_OSAMVP_MR.Rmd
      Validates the prioritized brain genes (CTD-2623N2.11, ECE2, FEZ2, GRPEL2,
      L3MBTL2) with the BrainMeta brain eQTL data against FinnGen R12 sleep apnea
      and against OSA MVP.

   BrainMeta_gene_sleepapnea_RadialMR.Rmd / BrainMeta_gene_OSAMVP_RadialMR.Rmd
      Re-run the same BrainMeta instruments with radial MR to detect outliers and
      to obtain the outlier-adjusted estimate.

   eQTLGen_gene_sleepapnea_MR.Rmd / eQTLGen_gene_OSAMVP_MR.Rmd
      Validates the prioritized blood genes with the eQTLGen whole blood eQTL data
      against FinnGen R12 sleep apnea and against OSA MVP, and saves the
      harmonised datasets in eQTLGen/harmonise_data/.

   eQTLGen_gene_sleepapnea_RadialMR.Rmd / eQTLGen_gene_OSAMVP_RadialMR.Rmd
      The corresponding radial MR sensitivity analyses for the blood genes.

   Iceland_pQTL_sleepapnea_MR.Rmd / Iceland_pQTL_OSAMVP_MR.Rmd
      Validates the prioritized plasma proteins (LCT, PCSK1, CALCOCO2) with the
      Iceland (deCODE) pQTL data against FinnGen R12 sleep apnea and against OSA
      MVP, and saves the harmonised datasets in Iceland/harmonise_data/.

   Iceland_pQTL_OSAMVP_RadialMR.Rmd
      The corresponding radial MR sensitivity analysis for the plasma proteins.

   External_OSA_MR-PRESSO_res.Rmd
      Collects the MR-PRESSO results of all validated targets for the FinnGen R12
      sleep apnea phenotype: the eQTLGen genes, the BrainMeta genes and the
      Iceland plasma proteins are read from the harmonised datasets, mr_presso()
      is applied to each of them and the causal estimate, the number of SNPs, the
      p value, the global test p value and the OR are written to
      External_OSA_MR-PRESSO_res.xlsx.

   External_OSAMVP_MR-PRESSO_res.Rmd
      The same MR-PRESSO summary for the OSA MVP phenotype.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In ExternalValidation_Code_Merged.Rmd those paths are rewritten to
short relative paths below the placeholder root Path/. Point that root at your own
data tree before running the code.

5. Software used in this folder

   - R 4.3.1 with TwoSampleMR, MendelianRandomization, MRPRESSO, RadialMR,
     mr.raps, dplyr, data.table, readxl, openxlsx, ggplot2 and ieugwasr
   - plink 1.90 (through plinkbinr) and the 1000 Genomes European reference panel
     for the LD clumping of the instruments

6. How to run

   1. The "harmonise" chunks must be run before the "MR", "RadialMR" and MR-PRESSO
      chunks: they create the *.RData harmonised datasets that the later chunks
      load.
   2. Put the QTL summary statistics and the two sleep apnea GWAS files below
      Path/datasource/ and the LD reference panel below Path/g1000/.
   3. The LD clumping step calls the LD reference through ieugwasr/plinkbinr; set
      the plink binary and the reference panel to your own installation if needed.
