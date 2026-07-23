# 목표
**BRAKER4로 annotation 완료된 데이터에서 특정 서열을 검색하여 annotation data의 상태 확인**
* luz gene block(발광 유전자) 구성요소
   * cph,hisps,h3h,luz
* illudin S 생합성에 사용되는 Omp4 gene cluster내의 유전자들
   * Omp4, cyp450, GATase1 anthranilate synthase, N-acetyltransferase, oxidoreductase, Omethyltransferase, Polygalactonurase
* OphMA 존재하는지 확인

**refrence 생물**
* ~~**O.olearus**: NCBI 내 luz gene(hisps 등) 데이터 존재하지 않음 -> 사용불가~~
* **Neonothopanus nambi**: O.japonicus genome의 luz gene 분석한 논문에서 차용됨. 이 생물로 검색 수행함

# workflow 및 사용 도구
```text

BRAKER4로 생성된 아미노산 서열(braker.aa)
-> NCBI에서 찾고자 하는 gene들의 아미노산 서열 검색
-> BLASTP에 BRAKER4 데이터와 NCBI에서 찾은 데이터를 넣고, 해당 아미노산 서열이 존재하는지 판별

```


# 분석 과정 중 사용된 데이터
### Neonothopanus nambi 유전체 데이터
[CPH](./N_nambi_CPH.md)\
[HispS](./N_nambi_HipS.md)\
[h3h](./N_nambi_h3h.md)\
[luz](./N_nambi_luz.md)

# 분석 과정
## 데이터 준비 및 BLAST DB 구축
가장 먼저 비교의 기준이 될 Query 서열(N.nambi)과 검색 대상이 될 DB 서열(O. japonicus)을 준비

1. Reference 서열(Query) 준비
NCBI Protein 데이터베이스에서 타겟 유전자 아미노산 서열 검색 및 다운로드\

2. BLAST DB 생성
터미널을 열고 BRAKER4 결과 파일이 있는 경로로 이동한 뒤, 아래 명령어를 입력하여 BLAST용 데이터베이스를 만듦(**braker.aa 사용**)
```Bash
makeblastdb -in braker.aa -dbtype prot -out O_japonicus_db
```


## BLASTP 검색 수행
```Bash
# luz gene (cph,hisps,h3h,luz) 검색
blastp -query N_nambi_bioluminescence_proteins.faa -db /panpyro/bravo/swkim/2019-nibr-biolum/LYS/output/O_japonicus/results/O_japonicus_db -outfmt "6 qseqid sseqid pident qcovs evalue" -evalue 1e-10 -out final_blast.txt
```

```Bash
# Best Hit 결과만 보기
(echo -e "Target_Gene\tO.japonicus_Gene\tIdentity(%)\tCoverage(%)\tE-value"; sort -k1,1 -k5,5g final_blast.txt | awk '!seen[$1]++') | column -t
```

## 결과
```text
# luz gene
Target_Gene                 O.japonicus_Gene  Identity(%)  Coverage(%)  E-value
sp|A0A3G9JYH7.1|LUZ_NEONM   g185.t1           84.483       87           7.67e-150
sp|A0A3G9JYJ6.1|CPH_NEONM   g12257.t1         62.963       97           1.62e-128
sp|A0A3G9K3K9.1|HIPS_NEONM  g243.t1           75.027       54           0.0
sp|A0A3G9K5C8.1|H3H_NEONM   g186.t1           62.061       100          0.0
```


# 결과 정리

