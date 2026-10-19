# Whole-exome sequencing variant analysis

This project documents a matched tumor/normal WES workflow built around nf-core/sarek 3.10.0. The sample sheet pairs HCC1395 tumor with HCC1395BL normal, and the scripts validate the metadata before launching the upstream pipeline.

## Inputs

The expected FASTQ files are listed in `config/samplesheet.csv` and are not stored in this repository. The file paths are for the analysis machine; update them to match your local input directory before running Sarek. Dataset provenance and sample roles are described in [metadata/DATASET.md](metadata/DATASET.md).

## Validate the project

The GitHub Actions workflow checks the repository structure, shell/Python syntax, and matched tumor-normal sample sheet. This validation does not run the full Sarek pipeline because the FASTQs and reference resources are not included in the repository.

```bash
bash -n scripts/run_sarek.sh
python -m py_compile scripts/validate_samplesheet.py scripts/summarize_variants.py
python scripts/validate_samplesheet.py --samplesheet config/samplesheet.csv
```

## Run Sarek locally

Install Nextflow, Docker, and Python 3. Place the four FASTQ files in the locations listed in the sample sheet, then run:

```bash
python scripts/validate_samplesheet.py --samplesheet config/samplesheet.csv --check-files
bash scripts/run_sarek.sh
```

The pipeline is pinned to nf-core/sarek 3.10.0 and uses the GRCh38 GATK reference configuration. The full workflow is computationally intensive and should be run on a machine with enough memory, storage, and compute for WES alignment and variant annotation.

## Results included here

The repository retains the project reports and compact process-level CSV summaries. Large generated outputs, including the annotated VCF and MultiQC HTML report, are not mirrored in this standalone copy. Use the complete analysis output directory from the machine that ran Sarek when those files are needed; do not treat the compact snapshots as a fresh run from GitHub Actions.

## Scope

This is a research workflow demonstration, not a clinical diagnostic pipeline. A successful samplesheet check only validates metadata and file references; it does not confirm the biological correctness of called variants.
