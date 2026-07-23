# QUAST
유전체 assembly 품질을 평가하는 도구  

assembled genome FASTA를 입력으로 받아서, assembly가 얼마나 길고 잘 이어져 있는지 통계로 보여줌.

1. quast용 가상 환경 만들기  
  ```text
  conda create -n quast_env -c conda-forge -c bioconda quast -y
  ```

2. Activate venv
```text
conda activate quast_env
```
3. Version check
```text
quast --version
```
If no error, next step

4. Run quast
```text
quast \
  ${PROJECT_DIR}/NIBRbiolumScaffold.fa \
  -o ${ANALYSIS_DIR}/quast_NIBR_biolum \
  -t 8 \
  --eukaryote
```
## Result
1. Open result
```text
cat ${ANALYSIS_DIR}/quast_NIBR_biolum/report.txt
```
### N50
긴 scaffold부터 더했을 때 전체 assembly 길이의 50%에 도달하게 만드는 scaffold의 길이
예시 결과 : N50 = 1,938,897 bp  

의미: 길이가 1,938,897 bp 이상인 긴 scaffold들을 합치면 전체 assembly 길이의 절반 이상을 차지한다는 뜻  
N50은 일반적으로 클수록 연속성이 높다.

### L50
전체 assembly 길이의 50%를 채우는 데 필요한 scaffold 개수  
예시 결과: L50 = 8

의미:가장 긴 scaffold 8개를 합하면 전체 assembly 길이의 50% 이상이 된다.  
L50은 일반적으로 작을수록 연속성이 높다.

### N90
긴 scaffold부터 더했을 때 전체 assembly 길이의 90%에 도달하게 만드는 scaffold의 길이  
예시 결과: N90 = 373,606 bp  

의미: 길이가 373,606 bp 이상인 scaffold들을 합하면 전체 assembly의 90% 이상을 차지한다.  
N90은 N50보다 항상 작거나 같다.

### L90
전체 assembly 길이의 90%를 채우는 데 필요한 scaffold 개수이다.  
예시 결과: L90 = 27  

의미: 가장 긴 scaffold 27개를 합하면 전체 assembly 길이의 90% 이상이 된다.

# DIAMOND Blastp
gene annotation을 4가지 발광 유전자에 대해 비교해야 한다. BRAKER4가 예측한 단백질 중 CPH, H3H, HispS, Luz와 상동성을 보이는 후보를 탐색하였다.

이를 위해 DIAMOND를 설치하고, BRAKER4 predicted protein FASTA로 검색용 database를 생성한다.

1. Create venv  
```text
conda create -n diamond_env -c conda-forge -c bioconda diamond -y
```
2. Activate venv
```text
conda activate diamond_env
```
3. version check
```text
which diamond
diamond version
```
4. Create a database in the working directory
```text
cd ${ANALYSIS_DIR}/GeneAnnotation

diamond makedb \
  --in ${PROJECT_DIR}/results/braker.aa \
  --db NIBR_biolum_BRAKER4
```
Normal message should be like this:
```text
diamond v2.2.4.184 (C) Max Planck Society for the Advancement of Science, Benjamin J. Buchfink, University of Tuebingen
Documentation, support and updates available at http://www.diamondsearch.org
Please cite: http://dx.doi.org/10.1038/s41592-021-01101-x Nature Methods (2021)

#CPU threads: N
Scoring parameters: (Matrix=BLOSUM62 Lambda=0.267 K=0.041 Penalties=11/1)
Database input file: ${PROJECT_DIR/}braker.aa
```
4. QC. Check whether abnormal amino acid character is included
```text
grep -v '^>' N_nambi_bioluminescence_proteins.faa \
  | tr -d '[:space:]' \
  | grep -o '[^ACDEFGHIKLMNPQRSTVWYXBZJUO*]' \
  | sort | uniq -c

grep -c '^>' N_nambi_bioluminescence_proteins.faa
grep -n '^>' N_nambi_bioluminescence_proteins.faa
grep -n '\\' N_nambi_bioluminescence_proteins.faa
```
5. Search
```
diamond blastp \
  --query N_nambi_bioluminescence_proteins.faa \
  --db NIBR_biolum_BRAKER4 \
  --out N_nambi_vs_BRAKER4.tsv \
  --outfmt 6 qseqid sseqid pident length qlen slen qstart qend sstart send evalue bitscore \
  --max-target-seqs 10 \
  --evalue 1e-10 \
  --threads 8
```
## Results
1. Check the relationship between protein, transcript, and gene IDs
```text
head -10 ${PROJECT_DIR}/JW/output/NIBR_biolum/results/braker.aa

zcat ${PROJECT_DIR}/JW/output/NIBR_biolum/results/braker.gff3.gz \
  | grep -v '^#' \
  | head
```
2. Select only best hit
```
awk '!seen[$1]++' N_nambi_vs_BRAKER4.tsv | column -t -s $'\t'

awk 'BEGIN{OFS="\t"}
!seen[$1]++ {
    qcov=($8-$7+1)/$5*100
    scov=($10-$9+1)/$6*100
    printf "%s\t%s\t%.1f\t%d\t%.1f\t%.1f\t%s\t%.1f\n",
    $1,$2,$3,$4,qcov,scov,$11,$12
}' N_nambi_vs_BRAKER4.tsv | column -t -s $'\t'
```
3. Calculate coverage
```
awk 'BEGIN{OFS="\t"}
{
  qcov=$4/$5*100;
  scov=$4/$6*100;
  print $1,$2,$3,$4,$5,$6,qcov,scov,$11,$12
}' N_nambi_vs_BRAKER4.tsv
```
- Query coverage: 넣은 기준 단백질이 얼마나 많이 정렬됐는지
- Subject coverage: BRAKER4가 예측한 후보 단백질이 얼마나 많이 정렬됐는지
