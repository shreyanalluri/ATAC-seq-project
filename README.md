---
title: "ATACseq Analysis Report"
author: "Your Name"
date: "Date"
output:
  html_document:
    toc: true
    toc_float: true
---

# ATACseq Analysis Report

## Methods

These fastq files were inspected for quality control using fastqc v0.12.1-0 with default parameters. Once QC checked, the reads were trimmed of adapters using trimmomatic v0.39 with default parameters. Bowtie2 v2.5.3 was used to create an index of the entire human genome using the GENCODE hg38 primary assembly for use in alignment, with default parameters. Alignment was also performed using Bowtie2 v2.5.3 with the -X 2000 flag, and the alignment output was converted to bam format using samtools v1.19.2 with default parameters. Alignments to the mitochondrial chromosome were removed using samtools v1.19.2 view with the egrep -v chrM flag.

After this filtering, reads were shifted to account for tagmentation process bias using deeptools v3.5.4 alignmentSieve with the --ATACshift flag with the GENCODE hg38 blacklist bed file. The R package ATACseqQC was used to determine fragment distribution sizes for all samples. Peak calling was performed on each replicate  using MACS3 callpeak with default parameters. Reproducible peaks were identified using bedtools v2.31.1 intersect with the -f 0.50 -r flag to select peaks that have at least 50% overlap between replicates. Signal-artifact regions were filtered from this bed file using the ENCODE blacklist with bedtools v2.31.1. 

The filtered peaks were annotated to their nearest genomic feature using HOMER v4.11 annotatepeaks with the GENCODE primary assembly gtf file. Motifs were identified using HOMER v4.11 findMotifsGenome with the GENCODE hg38 primary assembly fasta file. The annotated peaks were used to identify a list of proximal genes. This list of genes was used to perform functional enrichment analysis with Gene Ontology (GO).

## Questions to Address

1. Are there any concerning aspects of the quality control of your sequencing reads?
2. Are there any concerning aspects of the quality control related to alignment?
3. Based on all of your quality control, will you exclude any samples from further analysis?
4. Report the total number of alignments per sample
5. Report the number of alignments against the mitochondrial genome
6. How many peaks are present in each of the replicates?
7. How many peaks are present in your set of reproducible peaks? What strategy did you use to determine “reproducible” peaks?
8. How many peaks remain after filtering out peaks overlapping blacklisted regions?
9. Briefly discuss the main results of motif analysis and gene enrichment on peak annotations
10. What can chromatin accessibility let us infer biologically?

## Deliverables

1. Produce a fragment length distribution plot for each of the samples
2. Produce a table of how many alignments for each sample before and after filtering alignments falling on the mitochondrial chromosome
3. Create a signal coverage plot centered on the TSS (plotProfile) for the nucleosome-free regions (NFR) and the nucleosome-bound regions (NBR)
  - You may consider fragments (<100bp) to be those from the NFR and the rest as the NBR.
4. A table containing the number of peaks called in each replicate, and the number of reproducible peaks

5. A single BED file containing the reproducible peaks you determined from the experiment.

6. Perform motif finding on your reproducible peaks

  - Create a single table / figure with the most interesting results
7. Perform a gene enrichment analysis on the annotated peaks using a well


