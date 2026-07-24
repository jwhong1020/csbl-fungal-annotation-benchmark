# Helixer Gene Prediction Summary
## Analysis overview
Helixer는 `gff` 이외의 Gene prediction 결과 파일과 BUSCO 파일을 생성하지 않음.\
따라서 타 프로그램과 비교하기 위해서는 다른 분석 프로그램에 결과로 나온 `gff`를 넣어야 함.\
본 Summary에서는 GFF전용 통계 도구인 **AGAT**와 **BUSCO**를 사용하여 추가적으로 분석하였음

## Gene prediction results (AGAT)

| Metric                  |         Result |
| ----------------------- | -------------: |
| Genes                   |         15,304 |


### 결과(text)
```text
Reading /panpyro/bravo/swkim/2019-nibr-biolum/analysis/helixer/Omphalotus_japonicus_helixer.gff3
258492 line to process...

Progress : 100 %
Type (3rd column)	Number	Size total (kb)	Size mean (bp)	/!\Results are rounding to two decimal places 
cds	95234	18975.94	199.26
exon	98706	22324.26	226.17
five_prime_utr	17311	1412.26	81.58
gene	15304	27803.68	1816.76
mrna	15304	27803.68	1816.76
three_prime_utr	16567	1936.06	116.86
Total	258426	100255.88	387.95
Job done in 10 seconds
```

## Completeness assessment (BUSCO)
| Dataset  | Tool      | Complete | Fragmented | Missing |
| -------- | --------- | -------: | ---------: | ------: |
| Proteome   | BUSCO     |    88.1% |      7.8% |    4.0% |

### 결과(text)
```text
# BUSCO version is: 6.1.0 
# The lineage dataset is: fungi_odb12 (Creation date: 2026-05-22, number of genomes: 144, number of BUSCOs: 1122)
# BUSCO was run in mode: proteins

	***** Results: *****

	C:88.1%[S:86.9%,D:1.2%],F:7.8%,M:4.0%,n:1122	   
	989	Complete BUSCOs (C)			   
	975	Complete and single-copy BUSCOs (S)	   
	14	Complete and duplicated BUSCOs (D)	   
	88	Fragmented BUSCOs (F)			   
	45	Missing BUSCOs (M)			   
	1122	Total BUSCO groups searched		   

Dependencies and versions:
	hmmsearch: 3.4

```
