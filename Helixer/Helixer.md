# Helixer
## Helixer에 대해
딥러닝과 은닉 마르코프 모델(HMM)을 이용해 유전체 서열에서 annotation을 진행하는 프로그램

## 설치 및 구동 과정
### 설치
**Singularity 사용**
```bash
# pull current docker image 
singularity pull docker://gglyptodon/helixer-docker:latest

# fetch models, they will be downloaded into /home/<user>/.local/share
# unless specified otherwise
singularity run helixer-docker_latest.sif fetch_helixer_models.py

# download an example chromosome
wget ftp://ftp.ensemblgenomes.org/pub/plants/release-47/fasta/arabidopsis_lyrata/dna/Arabidopsis_lyrata.v.1.0.dna.chromosome.8.fa.gz

# test Helixer
singularity run --nv helixer-docker_latest.sif Helixer.py \
  --fasta-path Arabidopsis_lyrata.v.1.0.dna.chromosome.8.fa.gz --lineage land_plant \
  --gff-output-path Arabidopsis_lyrata_chromosome8_helixer.gff3
# notice '--nv' for GPU support
```


### 구동

```Bash
# run all Helixer components from fa to gff3
Helixer.py --lineage land_plant --fasta-path Arabidopsis_lyrata.v.1.0.dna.chromosome.8.fa.gz  \
  --species Arabidopsis_lyrata --gff-output-path Arabidopsis_lyrata_chromosome8_helixer.gff3
```

**1-step inference parameters**
|Parameter|Default|Explanation|
|:---|:---:|:---:|
|--fasta-path|/|FASTA input file|
|--gff-output-path|/|Output GFF3 file path|
|--species|/|Species name. Will be added to the GFF3 file.|
|--lineage|/|What model to use for the annotation. Options are: vertebrate, land_plant, fungi or invertebrate.|

### BUSCO 평가
