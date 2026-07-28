# 핵심 프로세스
전체 과정은 **[RNA-seq 맵핑] → [맵핑 결과물 조립] → [프로그램별 일치율 채점]** 순서로 진행됩니다.

이 분석을 마치고 `gffcompare` 리포트를 열어보면, 각 프로그램에 대해 아래와 같은 두 가지 핵심 수치를 얻게 됩니다. 

- **민감도 (Sensitivity, Sn):** 정답지(실제 RNA) 중에 이 프로그램이 얼마나 많이 맞췄는가? (이 수치가 낮으면 진짜 유전자를 많이 놓친 것입니다.)
- **특이도 (Specificity, Sp):** 이 프로그램이 예측한 유전자 중, 실제 RNA가 뒷받침해 주는 진짜 유전자의 비율은 얼마인가? (이 수치가 낮으면 컴퓨터가 가짜 유전자를 많이 찍어낸 것입니다.)

> **해석 예시:** A 프로그램이 2만 개의 유전자를 예측했지만 특이도(Sp)가 40%라면, 예측한 유전자 중 60%는 RNA-seq 증거가 전혀 없는 가짜라는 뜻입니다. 반면 B 프로그램은 1만 개를 예측했고 특이도가 90%라면, 비록 유전자 수는 적지만 훨씬 정확하고 현실적인 예측을 했다고 평가할 수 있습니다.
>

# 사용 프로그램
분석을 위해서는 DNA 염기서열 외에도 실험을 통해 얻은 RNA-seq 원시 데이터(Fastq 파일)가 필요합니다.

1. **STAR (또는 HISAT2)**
    - **역할 (Mapping):** 잘게 쪼개진 RNA-seq 리드(Fastq)들을 레퍼런스 유전체(Fasta)의 어느 위치에서 떨어져 나왔는지 찾아내어 지도 위에 붙여줍니다.
    - **결과물:** 정렬된 좌표가 기록된 `BAM` 파일.
2. **StringTie**
    - **역할 (Assembly):** STAR가 유전체 위에 붙여놓은 RNA 리드들을 이어 붙여서, 실제 세포 내에 존재하는 '진짜 전사체(Evidence Transcript)의 형태'를 조립해 냅니다. Annotation 프로그램들의 결과와 비교할 '표준 유전체'를 만드는 역할입니다.
    - **결과물:** RNA-seq 증거로만 만들어진 `GTF/GFF3` 파일.
3. **gffcompare**
    - **역할 (Benchmarking):** StringTie가 만든 정답지(실제 RNA)와, 각 예측 프로그램(Helixer, BRAKER4 등)이 만든 GFF3 파일을 겹쳐놓고 얼마나 일치하는지 채점합니다.
    - **결과물:** 정확도, 민감도 통계가 적힌 `.stats` 리포트 파일.

# 진행 과정 및 사용 코드 (수정 중)
## 패지키 설치 및 환경 조성
```bash
# Bioconda 채널 우선순위 설정(생략 가능)
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict #우선순위 적용 / 설치중 에러 방지

#annotation compare 전용 환경 생성 및 패키지 일괄 설치
conda create -n compare_env star stringtie gffcompare -y

#설치 확인 및 버전 테스트
conda activate compare_env

STAR --version
stringtie --version
gffcompare --version
```
하단의 프로세스 진행시 `compare_env` 환경에서 진행하면 됩니다.

## STAR
**1. 인덱스 생성**
```bash
# 1. 인덱스 생성 (한 번만 하면 됩니다)
STAR --runThreadN 25 \
     --runMode genomeGenerate \
     --genomeDir ./STAR_index \ #STAR_index 안에 파일 생성
     --genomeFastaFiles /PATH_게놈파일/assembly.fasta
```
`STAR_index` 폴더 안에 인덱스가 생성되었습니다. 이 인덱스는 polish 과정을 거치지 않은 데이터로 만든 것 입니다. 따라서 polish 이후 index를 다시 생성해야 한다면 다른 이름을 지정하고 여기에 기록으로 남겨주세요.

**2. RNA-seq 데이터 매핑**
이제, 각 샘플 별 RNA_seq 데이터들을 이용하여 mapping을 진행합니다.

각 8개의 mapping된 `BAM`파일들은 `mapping 1~8`로 명명되었으며 `STAR_mapping_results` 파일에 넣어두었습니다.

mapping 이름 뒤에 붙은 번호들은 sample들 뒤에 붙어있는 숫자들을 오름차순으로 배열하였을때의 순서와 동일합니다. 예시: `~1891`로 끝나는 샘플의 mapping 데이터는 `mapping1`입니다.

이 명명법은 이 후 과정을 for문으로 한번에 진행하기 위해 붙여졌습니다.

```bash
# 2. RNA-seq 데이터 매핑 (Pair-end 데이터 기준)
# R1 파일 8개를 쉼표로 묶고, 한 칸 띄운 뒤 R2 파일 8개를 쉼표로 묶습니다. (띄어쓰기 주의!)
STAR --runThreadN 25 \
     --genomeDir PATH_to-STAR_index/ \
     --readFilesIn Sample_1.fq.gz Sample_2.fq.gz \
     --readFilesCommand zcat \ #gz압축파일 읽어주는 명령어
     --outSAMtype BAM SortedByCoordinate \
     --outFileNamePrefix Path_to_STAR_mapping_results/mapping[숫자]_
```


## StringTie

## gffcompare


# 분석 결과
