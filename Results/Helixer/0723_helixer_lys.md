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

## Gene prediction results (AGAT_detailed)
위에서 사용한 함수와 다른 함수인 `agat_sp_statistics.pl`를 사용하여 더 구체적인 통계 생성

### 결과(text)
```text
Reading file helixer_completed.gff3
Parsing Finished
Compute statistics
Bye Bye.
--------------------------------------------------------------------------------

Compute mrna with isoforms if any

Number of genes                              15304
Number of mrnas                              15304
Number of mrnas with utr both sides          15294
Number of mrnas with at least one utr        15304
Number of cdss                               15304
Number of exons                              98706
Number of five_prime_utrs                    15304
Number of three_prime_utrs                   15294
Number of exon in cds                        95234
Number of exon in five_prime_utr             17311
Number of exon in three_prime_utr            16567
Number of intron in cds                      79930
Number of intron in exon                     83402
Number of intron in five_prime_utr           2007
Number of intron in three_prime_utr          1273
Number gene overlapping                      1825
Number of single exon gene                   1089
Number of single exon mrna                   1089
mean mrnas per gene                          1.0
mean cdss per mrna                           1.0
mean exons per mrna                          6.4
mean five_prime_utrs per mrna                1.0
mean three_prime_utrs per mrna               1.0
mean exons per cds                           6.2
mean exons per five_prime_utr                1.1
mean exons per three_prime_utr               1.1
mean introns in cdss per mrna                5.2
mean introns in exons per mrna               5.4
mean introns in five_prime_utrs per mrna     0.1
mean introns in three_prime_utrs per mrna    0.1
Total gene length                            27803679
Total mrna length                            27803679
Total cds length                             18975940
Total exon length                            22324260
Total five_prime_utr length                  1412259
Total three_prime_utr length                 1936061
Total intron length per cds                  5164633
Total intron length per exon                 5479419
Total intron length per five_prime_utr       164829
Total intron length per three_prime_utr      117304
mean gene length                             1816
mean mrna length                             1816
mean cds length                              1239
mean exon length                             226
mean five_prime_utr length                   92
mean three_prime_utr length                  126
mean cds piece length                        199
mean five_prime_utr piece length             81
mean three_prime_utr piece length            116
mean intron in cds length                    64
mean intron in exon length                   65
mean intron in five_prime_utr length         82
mean intron in three_prime_utr length        92
Longest gene                                 17906
Longest mrna                                 17906
Longest cds                                  15297
Longest exon                                 6438
Longest five_prime_utr                       636
Longest three_prime_utr                      676
Longest cds piece                            6054
Longest five_prime_utr piece                 636
Longest three_prime_utr piece                676
Longest intron into cds part                 4481
Longest intron into exon part                4481
Longest intron into five_prime_utr part      2762
Longest intron into three_prime_utr part     802
Shortest gene                                64
Shortest mrna                                64
Shortest cds                                 60
Shortest exon                                1
Shortest five_prime_utr                      1
Shortest three_prime_utr                     1
Shortest cds piece                           1
Shortest five_prime_utr piece                1
Shortest three_prime_utr piece               1
Shortest intron into cds part                30
Shortest intron into exon part               30
Shortest intron into five_prime_utr part     30
Shortest intron into three_prime_utr part    50

Re-compute mrna without isoforms asked. We remove shortest isoforms if any

Number of genes                              15304
Number of mrnas                              15304
Number of mrnas with utr both sides          15294
Number of mrnas with at least one utr        15304
Number of cdss                               15304
Number of exons                              98706
Number of five_prime_utrs                    15304
Number of three_prime_utrs                   15294
Number of exon in cds                        95234
Number of exon in five_prime_utr             17311
Number of exon in three_prime_utr            16567
Number of intron in cds                      79930
Number of intron in exon                     83402
Number of intron in five_prime_utr           2007
Number of intron in three_prime_utr          1273
Number gene overlapping                      1825
Number of single exon gene                   1089
Number of single exon mrna                   1089
mean mrnas per gene                          1.0
mean cdss per mrna                           1.0
mean exons per mrna                          6.4
mean five_prime_utrs per mrna                1.0
mean three_prime_utrs per mrna               1.0
mean exons per cds                           6.2
mean exons per five_prime_utr                1.1
mean exons per three_prime_utr               1.1
mean introns in cdss per mrna                5.2
mean introns in exons per mrna               5.4
mean introns in five_prime_utrs per mrna     0.1
mean introns in three_prime_utrs per mrna    0.1
Total gene length                            27803679
Total mrna length                            27803679
Total cds length                             18975940
Total exon length                            22324260
Total five_prime_utr length                  1412259
Total three_prime_utr length                 1936061
Total intron length per cds                  5164633
Total intron length per exon                 5479419
Total intron length per five_prime_utr       164829
Total intron length per three_prime_utr      117304
mean gene length                             1816
mean mrna length                             1816
mean cds length                              1239
mean exon length                             226
mean five_prime_utr length                   92
mean three_prime_utr length                  126
mean cds piece length                        199
mean five_prime_utr piece length             81
mean three_prime_utr piece length            116
mean intron in cds length                    64
mean intron in exon length                   65
mean intron in five_prime_utr length         82
mean intron in three_prime_utr length        92
Longest gene                                 17906
Longest mrna                                 17906
Longest cds                                  15297
Longest exon                                 6438
Longest five_prime_utr                       636
Longest three_prime_utr                      676
Longest cds piece                            6054
Longest five_prime_utr piece                 636
Longest three_prime_utr piece                676
Longest intron into cds part                 4481
Longest intron into exon part                4481
Longest intron into five_prime_utr part      2762
Longest intron into three_prime_utr part     802
Shortest gene                                64
Shortest mrna                                64
Shortest cds                                 60
Shortest exon                                1
Shortest five_prime_utr                      1
Shortest three_prime_utr                     1
Shortest cds piece                           1
Shortest five_prime_utr piece                1
Shortest three_prime_utr piece               1
Shortest intron into cds part                30
Shortest intron into exon part               30
Shortest intron into five_prime_utr part     30
Shortest intron into three_prime_utr part    50

--------------------------------------------------------------------------------



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
