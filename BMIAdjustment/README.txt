==============================================================================
BMI adjustment - MVMR and BMI based SMR
==============================================================================

1. What this folder is about

Body mass index (BMI) is the strongest known risk factor for obstructive sleep
apnea, so the prioritized targets are re-analysed with BMI taken into account.
Three groups of analyses are collected here.

   Multivariable MR (MVMR)   The prioritized gene or protein and BMI are used
                             together as the two exposures of a multivariable MR
                             analysis of sleep apnea, which shows whether the
                             target keeps its effect after the BMI effect is
                             accounted for.

   BMI-to-target MR          BMI (instrumented by the BMI GWAS) is used as the
                             exposure and the expression, the protein level or
                             sleep apnea itself is used as the outcome, which
                             shows the reverse direction of the BMI relationship.

   BMI based SMR             The prioritized genes and proteins are tested
                             against a BMI GWAS with SMR, which shows whether the
                             molecular trait is associated with BMI at all.

2. The merged code document

BMIAdjustment_Code_Merged.Rmd merges the 13 source files of this folder into a
single R Markdown document. Every source file became one code chunk: the file name
is given in the section heading, in the chunk title and in the first comment line
of the chunk, and the original chunk titles ("setting", "harmonise", "MR",
"RadialMR", "Read the blood mRNA SMR results", ...) are kept as comments. The two
shell scripts are kept in their own bash chunks. All chunks are set to eval = FALSE
so that the document can be knitted without the study data.

3. The code blocks and what they do

   MVMR analyses

   PlasmaPro_BMI_OSA_MVMR.Rmd
      MVMR of plasma proteins and BMI against obstructive sleep apnea. Chunk
      "blood gene" prepares the blood gene QTL files and runs the MVMR of the
      blood genes together with BMI; the two chunks "plasma protein" prepare the
      pQTL files of ARIC and of Iceland and run the MVMR of the plasma proteins
      together with BMI.

  BloodBrainGene_BMI_OSA_MVMR.Rmd
      MVMR of the blood and brain genes together with BMI against obstructive
      sleep apnea. Chunk "blood gene data" extracts the whole blood eQTL data,
      chunk "blood gene mvmr" runs the MVMR, chunk "brain gene data" extracts the
      brain frontal cortex BA9 eQTL data and chunk "brain gene mvmr" runs the MVMR
      of the brain genes.

   BloodeQTLGenGene_BMI_OSA_MVMR.Rmd
      MVMR of the eQTLGen blood genes together with BMI against obstructive sleep
      apnea. The chunks "cis-eQTL eQTLGen" and "trans-eQTL eQTLGen" prepare the
      cis and trans eQTL data with the allele frequency file, "blood cis-eQTL
      eQTLGen" and "blood trans-eQTL eQTLGen" write the per-gene input files,
      "bind cis and trans" merges the two sets and "blood eQTL eQTLGen mvmr" runs
      the MVMR.

   BrainBrainMetaGene_BMI_OSA_MVMR.Rmd
      MVMR of the BrainMeta brain genes together with BMI against obstructive
      sleep apnea; chunk "brain gene cis-eQTL data" prepares the cis-eQTL data and
      chunk "brain gene mvmr" runs the MVMR.

   BMI as the exposure (reverse direction MR)

   BMI_BloodGene_MR.Rmd
      MR of BMI on the expression of the prioritized blood genes. Chunk "setting"
      defines the targets and extracts the BMI instruments, chunk "harmonise"
      builds and saves the harmonised datasets in harmonise_data/, chunk "MR" runs
      the MR analysis with TwoSampleMR and mr.raps and chunk "RadialMR" repeats
      the analysis with radial MR to detect outliers.

   BMI_BrainGene_MR.Rmd
      The same MR analysis of BMI on the prioritized brain (frontal cortex BA9)
      gene expression.

   BMI_PlasmaPro_MR.Rmd
      The same MR analysis of BMI on the prioritized plasma protein levels (ARIC
      pQTL data).

   BMI_BrainPro_MR.Rmd
      The same MR analysis of BMI on the prioritized brain protein levels (ROSMAP
      pQTL data).

   BMI_OSA_MR.Rmd
      MR of BMI on obstructive sleep apnea (FinnGen R12), with the radial MR
      sensitivity analysis; this is the positive control of the BMI relationship
      of the study.

   BMI based SMR

   SMReQTL_bmi_code.txt   (shell)
      Runs SMR of a BMI GWAS against the blood and the brain eQTL besd files
      (--bfile g1000/EUR, --gwas-summary, --beqtl-summary, --maf 0.01) and then
      queries the probe-level eQTL data of the prioritized genes
      (Sig_Blood_SleepApnea_gene.txt, Sig_Brain_SleepApnea_gene.txt).

   SMReQTL_bmi_Rcode.Rmd
      Chunk "Read the blood mRNA SMR results" and chunk "Read the brain mRNA SMR
      results": read the BMI SMR output of the prioritized genes, add the beta and
      OR confidence intervals and keep the probes with p_SMR < 0.05 and
      p_HEIDI > 0.01.

   SMRpQTL_bmi_code.txt   (shell)
      Builds the plasma and brain protein besd files that are needed for the BMI
      SMR (--make-besd) and runs SMR of the BMI GWAS against them for the plasma
      and the brain proteins.

   SMRpQTL_bmi_Rcode.Rmd
      Chunk "Read the plasma protein SMR results" and chunk "Read the brain
      protein SMR results": collect the BMI SMR results of the prioritized
      proteins, add the beta and OR confidence intervals and keep the significant
      probes.

4. Path convention of the merged document

The original scripts refer to the analysis tree of the workstation on which they
were written. In BMIAdjustment_Code_Merged.Rmd those paths are rewritten to short
relative paths below the placeholder root Path/. Point that root at your own data
tree before running the code.

5. Software used in this folder

   - R 4.3.1 with TwoSampleMR, MendelianRandomization, RadialMR, mr.raps,
     dplyr, data.table, readxl, openxlsx and ggplot2
   - SMR 1.3.1 (smr-1.3.1-win.exe) and plink with the 1000 Genomes European
     reference panel

6. How to run

   1. Run the TWAS, SMR and external validation folders first: the BMI analyses
      reuse the prioritized target lists and the QTL files that those folders
      export.
   2. In the MR scripts the "harmonise" chunk must be run before the "MR" and
      "RadialMR" chunks, because it writes the harmonised datasets that the later
      chunks load.
