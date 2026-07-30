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

# 진행 과정 및 사용 코드 
## 패지키 설치 및 환경 조성
각 프로그램들의 환경 이름은 `star__env` `stringtie_env` `gffcompare_env` 입니다. 프로그램 사용 전 환경을 바꿔주세요.

```bash
# Bioconda 채널 우선순위 설정(생략 가능)
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict #우선순위 적용 / 설치중 에러 방지

#annotation compare 각 프로그램별 전용 환경 생성 및 패키지 일괄 설치
conda create -n star__env star -y
conda create -n stringtie_env stringtie -y
conda create -n gffcompare_env gffcompare -y

#설치 확인 및 버전 테스트
conda activate [사용할 환경]

STAR --version
stringtie --version
gffcompare --version
```



## STAR
#### 1. 인덱스 생성
```bash
# 1. 인덱스 생성 (한 번만 하면 됩니다)
STAR --runThreadN 25 \
     --runMode genomeGenerate \
     --genomeDir ./STAR_index \ #STAR_index 안에 파일 생성
     --genomeFastaFiles /PATH_게놈파일/assembly.fasta
```
`STAR_index` 폴더 안에 인덱스가 생성되었습니다. 이 인덱스는 polish 과정을 거치지 않은 데이터로 만든 것 입니다. 따라서 polish 이후 index를 다시 생성해야 한다면 다른 이름을 지정하고 여기에 기록으로 남겨주세요.

#### 2. RNA-seq 데이터 매핑
이제, 각 샘플 별 RNA_seq 데이터들을 이용하여 mapping을 진행합니다.
스크립트 작성 후 실행 권한을 주어야 돌아갑니다. (`chmod +x 스크립트이름.sh`) 


```bash
# 읽을 수 있는 파일 개수 늘리기
ultimate -n 65535
```

**STAR 명령어(개별 파일 돌릴때)** 

```bash
STAR --runThreadN 30 \
     --genomeDir PATH-to/STAR_index/ \
     --readFilesIn sample1_R1.fq,sample2_R1.fq\
     --readFilesCommand zcat \
     --outSAMtype BAM SortedByCoordinate \
     --outSAMstrandField intronMotif
     --outFileNamePrefix PATH-to/STAR_mapping_results/mapping[번호]_
```

**7개의 샘플의 STAR를 한번에 돌려주는 스크립트**

```bash
#!/bin/bash

# 1. STAR 환경(star__env) 작동
eval "$(conda shell.bash hook)"
conda activate star__env
# (만약 위 명령어로 환경이 안 켜지면 'source activate star__env' 로 수정해 주세요)

# 2. 오픈 파일 개수 제한 해제 (이전 오류 완벽 방지)
ulimit -n 65535

# 3. 경로 설정 (현재 폴더를 기준으로 코드 최소화)
RAW_DIR="PATH-to/01.RawData"
INDEX_DIR="../STAR_index"  # 현재 폴더(STAR_mapping_results) 바로 옆에 있으므로 상대경로 사용

echo "=========================================="
echo "Starting STAR Mapping Pipeline (7 Samples)"
echo "=========================================="

# 4. 분석 대상 샘플 수집 (오류 샘플 제외 및 오름차순 정렬)
# RAW_DIR 안의 Sample_ 폴더를 모두 찾되, grep -v 로 1898 샘플을 제외하고 sort로 정렬합니다.
SAMPLE_DIRS=($(ls -d ${RAW_DIR}/Sample_* | grep -v "Sample_TN1806R1898" | sort))

# 5. for문을 이용한 순차적 매핑
COUNT=1
for DIR in "${SAMPLE_DIRS[@]}"; do
    # 폴더 이름에서 'Sample_'을 떼어내고 진짜 일련번호만 추출 (예: TN1806R1891)
    SAMPLE_NAME=$(basename "$DIR" | sed 's/Sample_//')
    
    # 캡처본처럼 이름 뒤에 '--GATCAGAT' 같은 가변 문자열이 있을 수 있으므로 * 기호로 파일 탐색
    R1_FILE=$(ls ${DIR}/*_1.fq.gz)
    R2_FILE=$(ls ${DIR}/*_2.fq.gz)

    echo " "
    echo "[${COUNT}/7] Processing $SAMPLE_NAME ..."
    echo "  - R1: $R1_FILE"
    echo "  - R2: $R2_FILE"
    
    # STAR 실행
    STAR --runThreadN 30 \
         --genomeDir $INDEX_DIR \
         --readFilesIn $R1_FILE $R2_FILE \
         --readFilesCommand zcat \
         --outSAMtype BAM SortedByCoordinate \
         --outSAMstrandField intronMotif \
         --outFileNamePrefix mapping${COUNT}_

    # 카운트 증가
    COUNT=$((COUNT+1))
done

echo "=========================================="
echo "All mapping processes are successfully completed!"
echo "=========================================="
```


각 7개의 mapping된 `BAM`파일들은 `mapping 1~7`로 명명되었으며 `STAR_mapping_results` 파일에 넣어두었습니다.

⇒ **수정: Sample_TN1806R1898 서열이 잘못되어 제외하고 진행. 데이터는 7개만 만듦.**

mapping 이름 뒤에 붙은 번호들은 sample들 뒤에 붙어있는 숫자들을 오름차순으로 배열하였을때의 순서와 동일합니다.

⇒  `--outSAMstrandField intronMotif` 옵션 추가(07/29): 
이 옵션을 넣고 매핑을 돌리시면, STAR가 잘려 나간 흔적(GT/AG 등)을 보고 스스로 `+`인지 `-`인지 판단해서 BAM 파일에 `XS` 꼬리표를 달아줍니다.

## StringTie
**1. 개별 BAM 파일 조립 (Assembly):**

각 샘플의 BAM 파일을 독립적으로 조립합니다.

7개의 BAM 파일을 각각 StringTie에 넣어서 7개의 개별 GTF 파일을 만듭니다. 이때, for문을 사용하여 한번에 처리합니다.
```
# 7개 샘플을 차례대로 조립하는 for문
for i in {1..7}; do
    echo "Assembling mapping${i} ..."
    stringtie 경로_mapping${i}_Aligned.sortedByCoord.out.bam -p 32 -o 경로_sample${i}_assembly.gtf
done
```

*완료되면 `sample1_assembly.gtf`부터 `sample7_assembly.gtf`까지 총 7개의 파일이 생성됩니다.*

**2. 병합 리스트 파일(mergelist.txt) 만들기:**

병합할 파일들의 목록을 작성합니다.

StringTie에게 "어떤 파일들을 합칠지" 알려주기 위해, 방금 만든 7개의 GTF 파일 이름이 한 줄씩 적힌 텍스트 파일을 하나 만듭니다.

```
# 현재 폴더에 있는 모든 _assembly.gtf 파일 이름들을 mergelist.txt 안에 기록합니다.
ls *_assembly.gtf > mergelist.txt
```

`cat mergelist.txt`를 쳐서 7개 파일 이름이 잘 들어가 있는지 확인합니다.

**3. 최종 병합 (StringTie --merge):**

목록에 있는 파일들을 하나의 최종 정답지로 합칩니다.

`--merge` 옵션을 사용할 차례입니다. 앞서 만든 리스트 파일을 집어넣습니다.

```
stringtie --merge -p 32 -o stringtie_merged_evidence.gtf mergelist.txt
```

이 과정이 끝나면 생성되는 **`stringtie_merged_evidence.gtf`** 파일이 바로 7개 샘플의 발현 정보가 모두 깔끔하게 통합된 '단 하나의 궁극적인 정답지'가 됩니다.

## gffcompare
#### 비교 데이터 생성 스크립트
`bash` 파일로 생성. 구동은

```bash
#!/bin/bash

# 1. 기준이 되는 RNA-seq 정답지 (StringTie 결과물 경로 입력)
REF_GTF="/panpyro/bravo/swkim/2019-nibr-biolum/Annotation_compare/StringTie/stringtie_merged_evidence.gtf"

# 2. 평가할 6개 프로그램의 GFF3 파일 목록 배열
GFF_FILES=(
    "PATH-to/braker.gff3"
    "PATH-to/eviann.gff"
    "PATH-to/helixer.gff3"
    "PATH-to/tiberius_abinitio.gff3"
    "PATH-to/tiberius_evidence.gff3"
    "PATH-to/annevo.gff3"
)

# 3. 요약 결과를 저장할 파일 생성 (12개 컬럼)
SUMMARY_FILE="gffcompare_summary_all_levels.tsv"
echo -e "Program\tBase_Sn\tBase_Sp\tExon_Sn\tExon_Sp\tIntron_Sn\tIntron_Sp\tIntChain_Sn\tIntChain_Sp\tTrans_Sn\tTrans_Sp\tLocus_Sn\tLocus_Sp" > $SUMMARY_FILE

echo "=========================================="
echo "Starting FULL gffcompare evaluation..."
echo "=========================================="

# 4. for문을 이용한 일괄 분석
for GFF in "${GFF_FILES[@]}"; do
    PROGRAM_NAME=$(basename "$GFF")
    PROGRAM_NAME="${PROGRAM_NAME%.*}"
    PROGRAM_NAME=${PROGRAM_NAME/_completed/}

    echo "[Processing] $PROGRAM_NAME ..."

    if [ ! -f "$GFF" ]; then
        echo "  -> ERROR: File not found!"
        echo -e "${PROGRAM_NAME}\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-" >> $SUMMARY_FILE
        continue
    fi

    # gffcompare 실행 (필요 시 임시 strand 변환 코드를 여기에 적용하세요)
    gffcompare -r $REF_GTF -o $PROGRAM_NAME $GFF

    if grep -q "Transcript level" ${PROGRAM_NAME}.stats; then
        # 1. Base level ($3, $5)
        BASE_SN=$(grep "Base level" ${PROGRAM_NAME}.stats | awk '{print $3}')
        BASE_SP=$(grep "Base level" ${PROGRAM_NAME}.stats | awk '{print $5}')
        
        # 2. Exon level ($3, $5)
        EXON_SN=$(grep "Exon level" ${PROGRAM_NAME}.stats | awk '{print $3}')
        EXON_SP=$(grep "Exon level" ${PROGRAM_NAME}.stats | awk '{print $5}')
        
        # 3. Intron level ($3, $5)
        INTRON_SN=$(grep "Intron level" ${PROGRAM_NAME}.stats | awk '{print $3}')
        INTRON_SP=$(grep "Intron level" ${PROGRAM_NAME}.stats | awk '{print $5}')
        
        # 4. Intron chain level (단어가 3개라 위치가 $4, $6으로 한 칸씩 밀림)
        CHAIN_SN=$(grep "Intron chain level" ${PROGRAM_NAME}.stats | awk '{print $4}')
        CHAIN_SP=$(grep "Intron chain level" ${PROGRAM_NAME}.stats | awk '{print $6}')
        
        # 5. Transcript level ($3, $5)
        TRANS_SN=$(grep "Transcript level" ${PROGRAM_NAME}.stats | awk '{print $3}')
        TRANS_SP=$(grep "Transcript level" ${PROGRAM_NAME}.stats | awk '{print $5}')
        
        # 6. Locus level ($3, $5)
        LOCUS_SN=$(grep "Locus level" ${PROGRAM_NAME}.stats | awk '{print $3}')
        LOCUS_SP=$(grep "Locus level" ${PROGRAM_NAME}.stats | awk '{print $5}')
        
        # 탭(\t)으로 구분하여 한 줄로 출력
        echo -e "${PROGRAM_NAME}\t${BASE_SN}\t${BASE_SP}\t${EXON_SN}\t${EXON_SP}\t${INTRON_SN}\t${INTRON_SP}\t${CHAIN_SN}\t${CHAIN_SP}\t${TRANS_SN}\t${TRANS_SP}\t${LOCUS_SN}\t${LOCUS_SP}" >> $SUMMARY_FILE
    else
        echo "  -> WARNING: No data evaluated."
        echo -e "${PROGRAM_NAME}\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-\t-" >> $SUMMARY_FILE
    fi
done

echo "=========================================="
echo "All evaluations are done! Here is your summary:"
echo "=========================================="

# 5. 터미널에 최종 요약표 출력
cat $SUMMARY_FILE

```

# 분석 결과
Program	Base_Sn	Base_Sp	Exon_Sn	Exon_Sp	Intron_Sn	Intron_Sp	IntChain_Sn	IntChain_Sp	Trans_Sn	Trans_Sp	Locus_Sn	Locus_Sp
braker	66.2	87.9	52.4	56.3	76.9	76.7	9.6	16.9	9.6	15.7	22.7	21.5
eviann	56.6	94.6	47.8	82.1	59.3	94.7	17.2	41.4	17.2	40.3	35.6	54.7
helixer	65.6	79.3	41.3	49.2	57.4	62.6	4.5	8.7	4.5	8.1	10.9	8.1
tiberius_abinitio	52	85.1	40	60	59.4	80.4	7.3	17.2	7.2	16.1	17.4	16.1
tiberius_evidence	52	85.1	40	60	59.4	80.4	7.3	17.2	7.2	16.1	17.4	16.1
annevo	54	87	41	57.8	61.4	78.1	7	15.5	7	14.5	16.8	14.5
<img width="1873" height="316" alt="image" src="https://github.com/user-attachments/assets/e1d53378-ef13-448e-b445-be649343d45d" />






