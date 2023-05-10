# genome_assembly_pipeline

# nanopore_data_quality_check

This pipeline will perform the quality check for the long raw read sequencing data in the following steps:

1. Merging of the raw long read sequencing data, rawfile1 and rawfile2. More number of raw files can be added after rawfile2
2. Quality control check for the merged raw long read sequencing data using nanoQC       

# nanopore_data_filter_quality_check

This pipeline will perform the quality trimmimg followed by the quality check for the long raw read sequencing data in the following steps:

1. Merging of the raw long read sequencing data, rawfile1 and rawfile2. More number of raw files can be added after rawfile2
2. Quality-trimming and filtering of the merged raw long reads using nanofilt
3. Quality control check for the cleaned long read sequencing data using nanoQC

# nanopore_assembly_qc_checkm

This pipeline will make draft genome assembly from the raw long read sequencing data followed by the quality check of the assembly in the following steps:

1. This step is same as step 1 in "nanopore_data_filter_quality_check" pipeline
2. This step is same as step 2 in "nanopore_data_filter_quality_check" pipeline
3. Production of polished contigs through assembly of cleaned long read sequencing data using flye 
4. Assesment of completeness and contamination of the long read draft assembled genome in checkm

# nanopore_assembly_bin_qc_checkm

This pipeline will make draft genome assembly from the raw long read sequencing data followed by binning and quality checking of the bins in the following steps:

1. This step is same as step 1 in "nanopore_assembly_qc_checkm" pipeline
2. This step is same as step 2 in "nanopore_assembly_qc_checkm" pipeline
3. This step is same as step 3 in "nanopore_assembly_qc_checkm" pipeline
4. Binning of the long read draft genome assembly to check the lineage of the contigs using metabat2 
5. Assesment of completeness and contamination of the obtained bins in the long read draft genome assembly through checkm

# nanopore_assembly_bin_qc_busco

This pipeline will make draft genome assembly from the raw long read sequencing data followed by binning and quality checking of the required bin in the following steps:

1. This step is same as step 1 in "nanopore_assembly_bin_qc_checkm" pipeline
2. This step is same as step 2 in "nanopore_assembly_bin_qc_checkm" pipeline
3. This step is same as step 3 in "nanopore_assembly_bin_qc_checkm" pipeline
4. Binning of the long read draft genome assembly to check the lineage of the contigs using metabat2. Pipeline "nanopore_assembly_bin_qc_checkm" gives the statistics of different bins. The required bin name (bin_.2.fa) can be written as an output here.
5. Quantitative assessment of the appropriate bin (found in "nanopore_assembly_bin_qc_checkm" pipeline) in the long read draft genome assembly through busco. bin_2.fa is written as an example.

# nanopore_assembly_bin_consensus

This pipeline will make draft genome assembly from the raw long read sequencing data followed by binning and build a consensus sequence of the draft assembly for the appropriate bin in the following steps:

1. This step is same as step 1 in "nanopore_assembly_bin_qc_busco" pipeline
2. This step is same as step 2 in "nanopore_assembly_bin_qc_busco" pipeline
3. This step is same as step 3 in "nanopore_assembly_bin_qc_busco" pipeline
4. This step is same as step 4 in "nanopore_assembly_bin_qc_busco" pipeline
5. Building a consensus sequence from individual cleaned long sequencing reads (highQuality-reads.fastq) against a draft long read genome assembly in the appropriate bin (example: bin_2.fa, found from "nanopore_assembly_bin_qc_checkm" pipeline) using medaka_consensus

# Illumina_data_quality_check

This pipeline will perform the quality check for the short raw read paired-end sequencing data in the following step:

1. Quality control check for the raw short read paired-end sequencing data (R1 and R2) using fastqc

# Illumina_data_filter_quality_check

This pipeline will perform the quality trimmimg followed by the quality check for the short raw read paired-end sequencing data in the following steps:

1. Quality-trimming, filtering and adapter-trimming of the raw short read paired-end sequencing data (R1 and R2) using bbduk. More number of raw files can be added after rawfile2.
2. Quality control check for the cleaned short read paired-end sequencing data (cleaned R1 and R2) using fastqc

# nanopore_assembly_polishing_Illumina_data

This pipeline will perform polishing of the long read draft genome assembly using short paired-end read sequencing data in the following steps:

1. "nanopore_assembly_bin_consensus" pipeline and "Illumina_data_filter_quality_check" pipeline need to executed separately to obtain the consensus sequence from individual cleaned long sequencing reads against a draft long read genome assembly in the appropriate bin and the cleaned short read paired-end sequencing data, respectively.
2. Indexing of the consensus long read draft genome assembly using bwa index (consensus.fasta, obtained from the pipeline "nanopore_assembly_bin_consensus")
3. Alignment of the cleaned paired end short sequencing reads (R1 and R2) to all possible locations of the indexed consensus long read draft genome assembly using bwa mem. Individual sam files will be genearted. 
4. Filtering the alignments by reducing excess alignment, based on their insert size particularly near the edges of the repeat sequences to improve accuracy of polypolish  (following step) using polypolish_insert_filter.py 
5. Long read draft genome assembly and the alignments (filtered sam files) are given to polypolish which will output information to stderr and the polished assembly to stdout, so redirect its output to a file using polypolish:
6. Quantitative assessment of the polished genome assembly through busco where polypolish.fasta is written as an example.


# Modify the given path for input and output files at the required positions in the individual pipeline. The path /home/sutripa/pass/... is mentioned as an example.

# Software/package/tool required to install 
1. nanoqc
2. nanofilt
3. flye
4. checkm
5. metabat2
6. busco
7. medaka
8. bbduk
9. fastqc
10. bwa
11. polypolish
12. snakemake

# command to run the pipeline

snakemake --snakefile filename --core 40



