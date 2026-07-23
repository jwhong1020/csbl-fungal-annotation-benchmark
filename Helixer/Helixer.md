# Helixer
## Helixer에 대해
딥러닝과 은닉 마르코프 모델(HMM)을 이용해 유전체 서열에서 annotation을 진행하는 프로그램

### 입출력 데이터(Input & Output)
|구분|포맷|설명|
|:---|:---:|:---:|
|입력 (Input)|`.fasta` 또는 `.fa`|Assembled DNA 염기서열 데이터|
|출력 (Output)|`.gff3`|Annotation(exon/intron 좌표)된 유전체 데이터|

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
# 구동코드 (fa to gff3)
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
Helixer는 따로 BUSCO 평가를 수행하지 않음. 따라서 타 프로그램과의 성능을 비교하기 위해서는 따로 BUSCO를 수행해야함.
```Bash
# BUSCO 가상환경 세팅(단백질 추출을 위한 gffread 도구도 함께 설치)
conda create -n busco_env -c conda-forge -c bioconda busco gffread -y
conda activate busco_env

#Helixer GFF3에서 단백질 서열(FASTA) 추출
gffread -y <출력_파일_이름> -g <원본_DNA_genome.fasta 경로> <입력으로 들어가는 GFF3 파일 경로>

#BUSCO 실행 (Helixer 결과)
busco -i helixer_proteins.faa -l fungi_odb10 -o busco_result_helixer -m prot -c 10

```
