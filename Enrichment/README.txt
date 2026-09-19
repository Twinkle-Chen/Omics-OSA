==============================================================================
Enrichment - gene ontology analysis of the prioritized genes
==============================================================================

1. What this folder is about

This folder holds the functional enrichment step of the study. 
The prioritized blood and brain genes are submitted to an over
representation analysis of the Gene Ontology, and the enriched terms are
visualised as bubble plots that are faceted by GO category, so that the biological
processes and molecular functions behind the candidate targets become visible.

The enrichment result tables are read from the Excel files that the enrichment
analysis produced (GO_SA_Blood.xlsx for the blood genes and
GO_SA_Brain.xlsx for the brain genes); each table holds the description
of the term, the GO category, the gene count and the adjusted p value. The figures
are written as high resolution jpeg files.

2. The merged code document

Enrichment_Code_Merged.Rmd merges the single source file of this folder into an R
Markdown document. The source file became one code chunk whose title carries the
file name, and the original chunk titles are kept as comments inside the chunk.
The chunks are set to eval = FALSE so that the document can be knitted without the
study data.

3. The code blocks and what they do

   SA_ProGene_enrichment_Rcode.Rmd
      Chunk "Blood GO and KEGG enrichment analysis": loads the GO result table of
      the blood genes, sets the term order and the GO category order, defines the
      pathways that are printed in bold (the terms that are highlighted in the
      manuscript) and draws the bubble plot of the gene count against the adjusted
      p value, which is written to SA_Blood_bubble.jpg.
      Chunk "Brain GO and KEGG enrichment analysis": the same analysis and figure
      for the brain genes, written to SA_Brain_bubble.jpg.

4. Path convention of the merged document

The original script refers to the analysis tree of the workstation on which it was
written. In Enrichment_Code_Merged.Rmd that path is rewritten to a short relative
path below the placeholder root Path/; point it at your own working directory
before running the code.

5. Software used in this folder

   - R 4.3.1 with clusterProfiler, org.Hs.eg.db, DOSE, enrichplot, ggtext,
     ggnewscale, ggplot2, dplyr, readxl, openxlsx and AnnotationDbi

6. How to run

   1. Copy the two GO result tables (GO_SA_Blood.xlsx and
     GO_SA_Brain.xlsx) into the working directory of the merged document.
   2. Update the bold_terms vector if a different set of terms should be
      highlighted, then run the two chunks; each one writes one jpeg figure and
      leaves the plot object plot2 in the R session.
