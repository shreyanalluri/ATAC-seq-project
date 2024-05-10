# ATACseq

## Methods

The fastq files were inspected for quality control using fastqc v0.12.1-0 with default parameters. Once QC checked, the reads were trimmed of adapters using trimmomatic v0.39 with default parameters. Bowtie2 v2.5.3 was used to create an index of the entire human genome using the GENCODE hg38 primary assembly for use in alignment, with default parameters. Alignment was also performed using Bowtie2 v2.5.3 with the -X 2000 flag, and the alignment output was converted to bam format using samtools v1.19.2 with default parameters. 
The alignment bam files were sorted and indexed using samtools v1.19.2, following default parameters. Alignment quality control was performed using samtools v1.19.2 flagstats with default parameters. All QC reports generated thus far (FastQC and flagstats) were concatenated using multiqc v1.20 The sorted bam files were converted to BigWig format using deeptools v3.5.4 bamCoverage with default parameters. 

After this filtering, reads were shifted to account for tagmentation process bias using deeptools v3.5.4 alignmentSieve with the --ATACshift flag with the GENCODE hg38 blacklist bed file. The R package ATACseqQC was used to determine fragment distribution sizes for all samples. Peak calling was performed on each replicate  using MACS3 callpeak with default parameters. Reproducible peaks were identified using bedtools v2.31.1 intersect with the -f 0.50 -r flag to select peaks that have at least 50% overlap between replicates. Signal-artifact regions were filtered from this bed file using the ENCODE blacklist with bedtools v2.31.1. 

The filtered peaks were annotated to their nearest genomic feature using HOMER v4.11 annotatepeaks with the GENCODE primary assembly gtf file. Motifs were identified using HOMER v4.11 findMotifsGenome with the GENCODE hg38 primary assembly fasta file. The annotated peaks were used to identify a list of proximal genes. This list of genes was used to perform functional enrichment analysis with Gene Ontology (GO).

The score matrix for each replicate was computed using deeptools v3.5.4 computeMatrix and with reference genomic coordinates for the entire genome extracted from the UCSC Table Browser (selections: Clade: Mammal, Genome: Human, assembly: hg38, group: Genes and Gene Predictions, track: NCBI RefSeq, table: UCSC Refseq, region: genome). This score matrix was used to plot signal coverage for each replicate with deeptools v3.5.4 plotProfile, with default parameters.


