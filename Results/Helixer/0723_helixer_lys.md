# Helixer Gene Prediction Summary
## Analysis overview
Helixer는 gff 파일 이외의 Gene prediction 결과 파일과 BUSCO 파일을 생성하지 않음.\
따라서 타 프로그램과 비교하기 위해서는 다른 분석 프로그램에 결과로 나온 gff 파일을 넣어야 함.\
본 Summary에서는 GFF전용 통계 도구인 AGAT와 BUSCO를 사용하여 추가적으로 분석하였음

## Gene prediction results (AGAT)


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
