==============================================================================
Colocalization of the eQTL and pQTL signals with the obstructive sleep apnea GWAS
==============================================================================

1. What this folder is about

This folder holds the colocalization step of the study.
SMR and HEIDI cannot exclude linkage completely, so every prioritized gene and
protein is re-analysed with the Bayesian colocalization method coloc.abf to test
whether the molecular QTL and the sleep apnea GWAS share a single causal variant
(high posterior probability of H4).

For every target the cis-region is rebuilt from the eQTL or pQTL summary
statistics, harmonised against the FinnGen R12 sleep apnea GWAS (the alleles are
matched and the sign of the beta values is flipped where needed), the two datasets
are converted into coloc objects (case-control for sleep apnea, quantitative for
the QTL) and coloc.abf is run. The script finally stores the SNP with the highest
posterior probability (SNP.PP.H4) together with the five posterior probabilities
of the model.

Six scripts are provided, which differ in two ways:

   molecular layer   eQTL (blood and brain gene expression) or
                     pQTL (plasma and brain protein levels)
   cis window        100 kb, 250 kb or 500 kb around the target

The wide and narrow windows are used to show that the coloc result is stable
against the size of the cis-region, and the harmonised datasets that the scripts
export ("har/bloodgene/...", "har/..." csv files) are reused by the external
validation folder.

2. The merged code document

Colocalization_Code_Merged.Rmd merges the 6 source files of this folder into a
single R Markdown document. Every source file became one code chunk: the file name
is given in the section heading, in the chunk title and in the first comment line
of the chunk, and the original chunk titles ("Blood genes", "Brain genes", "Plasma
proteins", "Brain proteins") are kept as comments. All chunks are set to
eval = FALSE so that the document can be knitted without the study data.

3. The code blocks and what they do

   eQTL_SleepApnea_colocalization_100kb.Rmd
      Chunk "Blood genes": reads the SMR significant blood genes
      (Sig_Blood_SleepApnea.xlsx of the SMR section), loads the whole blood eQTL
      data of each gene, keeps the sleep apnea GWAS variants within 100 kb of the
      target, harmonises both datasets and runs coloc.abf. Writes
      BloodGene_SleepApnea_colocalization_100kb.xlsx.
      Chunk "Brain genes": the same for the brain (frontal cortex BA9) eQTL data,
      with the 100 kb window.

   eQTL_SleepApnea_colocalization_250kb.Rmd
      The same two analyses with a 250 kb cis window, to check that the
      colocalization result does not depend on the window size.

   eQTL_SleepApnea_colocalization_500kb.Rmd
      The same two analyses with a 500 kb cis window.

   pQTL_SleepApnea_colocalization_100kb.Rmd
      Chunk "Plasma proteins": reads the SMR significant plasma proteins
      (Sig_Plasma_SleepApnea.xlsx), loads the ARIC plasma pQTL data and runs
      coloc.abf in a 100 kb window around each target. Writes
      PlasmaPro_SleepApnea_colocalization_100kb.xlsx.
      Chunk "Brain proteins": the same for the ROSMAP brain pQTL data.

   pQTL_SleepApnea_colocalization_250kb.Rmd
      The same two analyses with a 250 kb cis window.

   pQTL_SleepApnea_colocalization_500kb.Rmd
      The same two analyses with a 500 kb cis window.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In Colocalization_Code_Merged.Rmd those paths are rewritten to short
relative paths below the placeholder root Path/. Point that root at your own data
tree before running the code.

5. Software used in this folder

   - R 4.3.1 with coloc, TwoSampleMR, MendelianRandomization, dplyr, data.table,
     readxl, openxlsx, stringr, locuscomparer and WhopGenome
   - plink 1.90 (through plinkbinr) and the 1000 Genomes European reference panel

6. How to run

   1. Run the SMR folder first: the colocalization scripts read the significant
      probes that SMR exports, and they expect the probe-level QTL files that the
      SMR shell scripts write.
   2. Put the QTL summary statistics and the sleep apnea GWAS below
      Path/datasource/.
   3. Run the six chunks of the merged document in the order in which they appear;
      each one writes its own summary workbook.
