
<img width="1426" height="971" alt="image" src="https://github.com/user-attachments/assets/2dde3dec-99c3-43bd-a57b-1f549b1225a3" />






# Whole-Exom-Variant-Analysis
---

## Pipeline Overview

| Step                  | Tool(s)                                                            | Description                                                                                  |
|------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **1. QC**                 | FastQC → MultiQC                                                   | Quality control and reporting of raw FASTQ files                                            |
| **2. Alignment**          | BWA-MEM2 → Samtools                                               | Alignment to reference genome, sorting, indexing                                            |
| **3. Germline Calling**   | GATK HaplotypeCaller → GenotypeGVCFs → VQSR/Hard filter            | gVCF-based calling for single-sample and cohort-level                                      |
| **4. Somatic Calling**    | Mutect2 → FilterMutectCalls *(+ optional Strelka2)*               | Tumor/Normal paired variant calling                                                         |
| **5. Annotation**         | VEP *(or snpEff)*                                                 | Functional annotation of variants                                                           |


## Conda Environments (envs)

| Env File                 | Tools / Packages             |
| ------------------------ | ---------------------------- |
| `envs/bwa_samtools.yaml` | bwa-mem2, samtools           |
| `envs/gatk.yaml`         | GATK4 (≥4.3), java, samtools |
| `envs/picard.yaml`       | Picard                       |
| `envs/fastqc.yaml`       | FastQC                       |
| `envs/bcftools.yaml`     | bcftools                     |
| `envs/vep.yaml`          | Ensembl VEP + plugins        |
| `envs/multiqc.yaml`      | MultiQC                      |


## Run the Full Workflow (Local)

snakemake --use-conda -s Snakefile --configfile config.yaml 

## Outputs

| Directory             | Content                            |
| --------------------- | ---------------------------------- |
| `results/qc/`         | FastQC + MultiQC reports           |
| `results/bam/`        | Aligned, sorted, indexed BAM files |
| `results/germline/`   | gVCFs, joint-called VCFs           |
| `results/somatic/`    | Somatic VCFs                       |
| `results/annotation/` | Annotated VCFs (VEP / snpEff)      |


## References:

GATK Best Practices : https://gatk.broadinstitute.org/

VEP (Variant Effect Predictor) : https://www.ensembl.org/info/docs/tools/vep/




