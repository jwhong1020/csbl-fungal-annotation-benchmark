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
| Proteome   | BUSCO     |    90.2% |       4.0% |    5.8% |

### 결과(text)
```text
# BUSCO version is: 6.1.0 
# The lineage dataset is: fungi_odb10 (Creation date: 2024-01-08, number of genomes: 549, number of BUSCOs: 758)
# BUSCO was run in mode: proteins

	***** Results: *****

	C:90.2%[S:89.2%,D:1.1%],F:4.0%,M:5.8%,n:758	   
	684	Complete BUSCOs (C)			   
	676	Complete and single-copy BUSCOs (S)	   
	8	Complete and duplicated BUSCOs (D)	   
	30	Fragmented BUSCOs (F)			   
	44	Missing BUSCOs (M)			   
	758	Total BUSCO groups searched		   

Dependencies and versions:
	hmmsearch: 3.4
```
