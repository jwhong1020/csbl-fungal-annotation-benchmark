## 1. Installation
Assume you are working at the root directory of your project.  
1. Create folder
   ```text
   mkdir -p software
   cd software
   ```
2. Get key
3. Download the sister proteome
  ```text
singularity exec \
  --bind ${PROJECT_DIR}:/project \
  ${PROJECT_DIR}/software/FunGAP/docker/sandbox \
  python /workspace/FunGAP/download_sister_orgs.py \
  --download_dir /project/analysis/fungap/input/sister_proteome/sister_orgs \
  --taxon omphalotus \
  --num_sisters 3 \
  --email_address my@gmail.com
  ```
4. AUGUSTUS species selection
```text
python3 ${PROJECT_DIR}/software/FunGAP/get_augustus_species.py \
  --genus_name Omphalotus \
  --email_address my@gmail.com
```
AUGUSTUS species model result = coprinus_cinereus

5. Combine sister proteome files into one
6. Final run
   ```text
   singularity exec \
  --bind ${PROJECT_DIR}:/project \
  ${PROJECT_DIR}/software/FunGAP/docker/sandbox \
  /workspace/FunGAP/fungap.py \
  --output_dir /project/analysis/fungap/output \
  --trans_read_1 /project/fastpQC_1.fastq.gz \
  --trans_read_2 /project/fastpQC_2.fastq.gz \
  --genome_assembly /project/NIBRbiolumScaffold.fa \
  --augustus_species coprinus_cinereus \
  --sister_proteome /project/prot_db.faa \
  --num_cores 25 \
  --busco_dataset fungi_odb12.2
  ```
