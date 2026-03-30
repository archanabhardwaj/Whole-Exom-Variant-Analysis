# WGS/WES Germline + Somatic Variant Analysis Snakemake Pipeline

This repository contains a production-ready, modular **Snakemake** pipeline for combined **WGS/WES germline and somatic** variant analysis. It includes:

- `Snakefile` (main workflow)
- `config.yaml` (configurable inputs / parameters)
- `samples.tsv` (expected sample sheet format)
- `envs/` (conda environment YAMLs used by rules)
- `scripts/` (small helper scripts)
- `README.md` (run examples, notes)

```python
# Snakefile (top-level)
# - Supports paired-end FASTQs
# - QC: FastQC -> MultiQC
# - Germline path: BWA -> markdup -> BQSR -> HaplotypeCaller gVCF -> GenotypeGVCFs -> VQSR/Filter
# - Somatic path: BWA -> markdup -> Mutect2 -> FilterMutectCalls -> optional Strelka2
# - Annotation: VEP (or snpEff)





## README / Run examples

1. Create conda env for snakemake (recommended):

```bash
mamba create -n snakemake -c conda-forge -c bioconda snakemake mamba
conda activate snakemake
```

2. Run the workflow (local, 24 threads):

```bash
snakemake --use-conda -s Snakefile -j 24 --configfile config.yaml --printshellcmds
```

3. Run a single rule for debugging:

```bash
snakemake --use-conda -s Snakefile align --cores 4
```

---



## scripts/

- `scripts/parse_samples.py` — optional helper to validate `samples.tsv` and produce sample lists for downstream rules.



## envs/

- `envs/bwa_samtools.yaml` — bwa-mem2 + samtools
- `envs/gatk.yaml` — GATK4 (4.3+), java, samtools
- `envs/picard.yaml` — picard
- `envs/fastqc.yaml` — fastqc
- `envs/bcftools.yaml` — bcftools
- `envs/vep.yaml` — ensembl VEP and plugin dependencies
- `envs/multiqc.yaml` — multiqc



(Each env YAML contains the minimal packages required; customize channels and versions.)




## Notes, recommendations & customization

- WES vs WGS: for WES, pass `exome_intervals` (BED/interval_list) into `HaplotypeCaller`/`Mutect2` using `-L` to restrict calling.
- Variant recalibration (VQSR) is recommended for large cohorts (>=30 samples for SNPs); hard-filtering shown as example otherwise.
- Somatic calling: Mutect2 is used here as default. Consider running Strelka2 and combining calls, or running Lancet, VarDict for additional sensitivity.
- Panel of Normals (PON): build with many normal WGS/WES to reduce artifacts.
- Resource files: make sure to provide correct GRCh37/GRCh38-matching resources (dbSNP, gnomAD, Mills/1000G indels etc.).
- Indexing: pipeline assumes FASTA has .fai and dict; if missing, create them with `samtools faidx` and `gatk CreateSequenceDictionary`.

---

