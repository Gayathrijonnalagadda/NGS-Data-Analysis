# NGS Analysis of Multi-Drug Resistant E. coli
## Overview
**This repository contains the Next-Generation Sequencing (NGS) data analysis workflow for _whole genome sequence_ (WGS) multi-drug resistant Escherichia coli. The analysis follows a structured pipeline from raw data acquisition to quality control. More steps will be added as the analysis progresses**

### Steps Completed So Far:
#### System Preparation
Updated Linux system using:
```
sudo apt update && sudo apt upgrade -y
```
**Resolved dependencies and ensured proper package installations**
#### Installing OpenJDK (for bioinformatics tools compatibility)
Installed OpenJDK using:
```
sudo apt install openjdk-11-jdk -y
```
#### Downloading Whole Genome Reads
Retrieved sequencing data for multi-drug resistant E. coli from public repositories.
* Used SRA Toolkit to download raw reads:
```
prefetch SRR31548417
```
* Converted .sra format to FASTQ files:
```
fastq-dump --split-files SRR31548417
```
#### Quality Control (QC)
Performed FastQC analysis to check read quality:
```
mkdir qc_reports
fastqc SRR31548417_1.fastq.gz SRR31548417_2.fastq.gz -o qc_reports/
```
Stored all quality reports in qc_reports/ directory.

### Next Steps
1 Adapter trimming and quality filtering using Trimmomatic.

2 Genome assembly using SPAdes.

3 Read mapping to a reference genome.

4 Variant calling and functional annotation.

5 Antibiotic resistance gene identification.

This README will be updated as more steps are completed. Stay tuned! 🚀
