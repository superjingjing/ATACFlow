# NCBI-Hackathons/ATACFlow
This pipeline performs ATAC Seq using Nextflow

This document describes the output produced by the pipeline. Most of the plots are taken from the MultiQC report, which summarises results at the end of the pipeline.

## Pipeline overview
The pipeline is built using [Nextflow](https://www.nextflow.io/)
and processes data using the following steps:

* [FastQC](#fastqc) - read quality control
* [MultiQC](#multiqc) - aggregate report, describing results of the whole pipeline

## FastQC
[FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) gives general quality metrics about your reads. It provides information about the quality score distribution across your reads, the per base sequence content (%T/A/G/C). You get information about adapter contamination and other overrepresented sequences.

For further reading and documentation see the [FastQC help](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/).

**Output directory: `results/fastqc`**

* `sample_fastqc.html`
  * FastQC report, containing quality metrics for your untrimmed raw fastq files
* `zips/sample_fastqc.zip`
  * zip file containing the FastQC report, tab-delimited data file and plot images

## MultiQC
[MultiQC](http://multiqc.info) is a visualisation tool that generates a single HTML report summarising all samples in your project. Most of the pipeline QC results are visualised in the report and further statistics are available in within the report data directory.

**Output directory: `results/multiqc`**

* `Project_multiqc_report.html`
  * MultiQC report - a standalone HTML file that can be viewed in your web browser
* `Project_multiqc_data/`
  * Directory containing parsed statistics from the different tools used in the pipeline

For more information about how to use MultiQC reports, see http://multiqc.info


## TrimGalore

TrimGalore is used for removal of adapter contamination and trimming of low quality regions. TrimGalore uses Cutadapt for adapter trimming and runs FastQC after it finishes.

MultiQC reports the percentage of bases removed by TrimGalore in the General Statistics table, along with a line plot showing where reads were trimmed.

Output directory: results/trimgalore

Contains FastQ files with quality and adapter trimmed reads for each sample, along with a log file describing the trimming.

    sample_val_1.fq.gz, sample_val_2.fq.gz
        Trimmed FastQ data, reads 1 and 2.
    sample_val_1.fastq.gz_trimming_report.txt
        Trimming report (describes which parameters that were used)
    sample_val_1_fastqc.html
    sample_val_1_fastqc.zip
        FastQC report for trimmed reads

Single-end data will have slightly different file names and only one FastQ file per sample:

    sample_trimmed.fq.gz
        Trimmed FastQ data
    sample.fastq.gz_trimming_report.txt
        Trimming report (describes which parameters that were used)
    sample_trimmed_fastqc.html
    sample_trimmed_fastqc.zip
        FastQC report for trimmed reads


## bowtie2
[bowtie2](http://bowtie-bio.sf.net/bowtie2)
