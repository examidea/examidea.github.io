# FastQC

### Setup with Miniforge/Mamba

#### Install and initialize
- **Install Miniforge:** Download from the official site for your OS, then run the installer.
- **Initialize shell:**  
  ```
  conda init bash
  source ~/.bashrc
  ```
- **Install mamba (faster conda):**  
  ```
  conda install -n base -c conda-forge mamba
  ```

#### Create a clean environment
- **Create env with core tools:**  
  ```
  mamba create -n illumina-wgs -c bioconda -c conda-forge \
    fastqc multiqc fastp bbmap bwa samtools seqtk \
    jellyfish genomescope2 kraken2 bracken
  ```
- **Activate it:**  
  ```
  conda activate illumina-wgs
  ```

---

### Project structure

- **Make folders:**  
  ```
  mkdir -p project/{raw,qc,trimmed,logs}
  ```
- **Place files:** Put your `R1.fastq.gz` and `R2.fastq.gz` into `project/raw/`.

---

### Quick sanity checks

- **Check file integrity:**  
  ```
  seqtk fqchk project/raw/R1.fastq.gz > project/logs/R1.fqchk.txt
  seqtk fqchk project/raw/R2.fastq.gz > project/logs/R2.fqchk.txt
  ```
- **Peek at headers (optional):**  
  ```
  zcat project/raw/R1.fastq.gz | head -n 8
  ```

---

### Read quality control (QC)

#### Run FastQC
- **Per‑sample QC:**  
  ```
  fastqc -o project/qc -t 8 project/raw/R1.fastq.gz project/raw/R2.fastq.gz
  ```

#### Summarize with MultiQC
- **Aggregate reports:**  
  ```
  multiqc project/qc -o project/qc
  ```
- **Interpretation:**
  - **Per‑base quality:** Should be high across cycles; dips at the end are common.
  - **Adapter content:** If present, trim.
 
