
# NGS Analysis of Multi-Drug Resistant *E. coli*

## Overview
This repository contains the Next-Generation Sequencing (NGS) data analysis workflow for whole genome sequencing (WGS) of multi-drug resistant *Escherichia coli*. The analysis follows a structured pipeline from raw data acquisition to quality control, genome assembly, read mapping, and variant calling. More steps will be added as the analysis progresses.

---
## Steps Completed So Far

### **1️⃣ System Preparation**
- Updated Linux system:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
- Installed dependencies and ensured proper package installations.
- Installed OpenJDK (required for bioinformatics tools compatibility):
  ```bash
  sudo apt install openjdk-11-jdk -y
  ```

### **2️⃣ Downloading Whole Genome Reads**
- Retrieved sequencing data for multi-drug resistant *E. coli* from public repositories.
- Used **SRA Toolkit** to download raw reads:
  ```bash
  prefetch SRR31548417
  ```
- Converted `.sra` format to FASTQ files:
  ```bash
  fastq-dump --split-files SRR31548417
  ```

### **3️⃣ Quality Control (QC)**
- Performed **FastQC** analysis to check read quality:
  ```bash
  mkdir qc_reports
  fastqc SRR31548417_1.fastq.gz SRR31548417_2.fastq.gz -o qc_reports/
  ```
- Stored all quality reports in the `qc_reports/` directory.

### **4️⃣ Genome Assembly Using SPAdes**
- Used **SPAdes** to assemble the genome from paired-end reads:
  ```bash
  spades.py -1 SRR31548417_1.fastq.gz -2 SRR31548417_2.fastq.gz -o spades_output
  ```
- Verified output and checked for `scaffolds.fasta`.

### **5️⃣ Reference Genome Preparation**
- Downloaded the reference genome from NCBI (in `ncbi_dataset.zip`).
- Extracted and indexed the reference genome:
  ```bash
  unzip ncbi_dataset.zip -d ncbi_reference
  cp ncbi_reference/ncbi_dataset/data/*/genomic.fna reference.fasta
  bwa index reference.fasta
  samtools faidx reference.fasta
  ```

### **6️⃣ Read Mapping to Reference Genome**
- Mapped reads to the reference genome using **BWA-MEM**:
  ```bash
  bwa mem -t 8 reference.fasta SRR31548417_1.fastq.gz SRR31548417_2.fastq.gz > mapped_reads.sam
  ```
- Converted, sorted, and indexed the BAM file:
  ```bash
  samtools view -Sb mapped_reads.sam > mapped_reads.bam
  samtools sort mapped_reads.bam -o mapped_reads_sorted.bam
  samtools index mapped_reads_sorted.bam
  ```

### **7️⃣ Variant Calling**
- Used **FreeBayes** for variant calling:
  ```bash
  freebayes -f reference.fasta mapped_reads_sorted.bam > variants.vcf
  ```

---
## **Next Steps** 🚀
1️⃣ **Antibiotic resistance gene identification** using CARD or AMRFinderPlus.  
2️⃣ **Phylogenetic analysis** to determine strain relationships.  
3️⃣ **Comparative genomics** to analyze mutations relative to other strains.  
4️⃣ **Functional analysis of resistance genes**.  

This README will be updated as more steps are completed. Stay tuned! 🎯

