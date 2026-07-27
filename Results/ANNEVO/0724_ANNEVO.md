# ANNEVO Gene Prediction Summary

## Analysis overview
- **Sample:** NIBR_biolum
- **Pipeline:** ANNEVO
- **Threads:** 16
- **Genome:** `assembly.fa`
- **RNA-Seq evidence:** Paired-end RNA-Seq

## Input data
| Input | File |
|---|---|
| Genome assembly | `assembly.fasta` |
| model | `ANNEVO_Fungi` |

## Gene prediction results

| Metric | Result |
| --- | ---: |
| Gene models | 13,108 |
| Transcripts | 13,108 |
| Transcripts per gene | 1.00 |
| Exons | 84,564 |
| CDS features | ? |
| Mean exons per transcript | 6.5 |
| Single-exon transcripts | 820 |
| Multi-exon transcripts | 12,288 |

#### text
```txt
Reading file annevo_completed.gff3
Parsing Finished
Compute statistics
Bye Bye.
--------------------------------------------------------------------------------

Compute mrna with isoforms if any

Number of genes                              13108
Number of mrnas                              13108
Number of cdss                               13108
Number of exons                              84564
Number of exon in cds                        84564
Number of intron in cds                      71456
Number of intron in exon                     71456
Number gene overlapping                      57
Number of single exon gene                   820
Number of single exon mrna                   820
mean mrnas per gene                          1.0
mean cdss per mrna                           1.0
mean exons per mrna                          6.5
mean exons per cds                           6.5
mean introns in cdss per mrna                5.5
mean introns in exons per mrna               5.5
Total gene length                            20980727
Total mrna length                            20980727
Total cds length                             16758855
Total exon length                            16758855
Total intron length per cds                  4221872
Total intron length per exon                 4221872
mean gene length                             1600
mean mrna length                             1600
mean cds length                              1278
mean exon length                             198
mean cds piece length                        198
mean intron in cds length                    59
mean intron in exon length                   59
Longest gene                                 11977
Longest mrna                                 11977
Longest cds                                  10713
Longest exon                                 4010
Longest cds piece                            4010
Longest intron into cds part                 1984
Longest intron into exon part                1984
Shortest gene                                21
Shortest mrna                                21
Shortest cds                                 21
Shortest exon                                2
Shortest cds piece                           2
Shortest intron into cds part                20
Shortest intron into exon part               20

Re-compute mrna without isoforms asked. We remove shortest isoforms if any

Number of genes                              13108
Number of mrnas                              13108
Number of cdss                               13108
Number of exons                              84564
Number of exon in cds                        84564
Number of intron in cds                      71456
Number of intron in exon                     71456
Number gene overlapping                      57
Number of single exon gene                   820
Number of single exon mrna                   820
mean mrnas per gene                          1.0
mean cdss per mrna                           1.0
mean exons per mrna                          6.5
mean exons per cds                           6.5
mean introns in cdss per mrna                5.5
mean introns in exons per mrna               5.5
Total gene length                            20980727
Total mrna length                            20980727
Total cds length                             16758855
Total exon length                            16758855
Total intron length per cds                  4221872
Total intron length per exon                 4221872
mean gene length                             1600
mean mrna length                             1600
mean cds length                              1278
mean exon length                             198
mean cds piece length                        198
mean intron in cds length                    59
mean intron in exon length                   59
Longest gene                                 11977
Longest mrna                                 11977
Longest cds                                  10713
Longest exon                                 4010
Longest cds piece                            4010
Longest intron into cds part                 1984
Longest intron into exon part                1984
Shortest gene                                21
Shortest mrna                                21
Shortest cds                                 21
Shortest exon                                2
Shortest cds piece                           2
Shortest intron into cds part                20
Shortest intron into exon part               20

--------------------------------------------------------------------------------


```


## BUSCO Assessment
Used fungi_odb 12
| Dataset | Complete | Single-copy | Duplicated | Fragmented | Missing |
| ------------------------ | -------- | ----------- | ---------- | ---------- | ------- |
| Proteome | 89.8% | 88.3% | 1.4% | 6.5% | 3.7% |

#### text
```text
# BUSCO version is: 6.1.0 
# The lineage dataset is: fungi_odb12 (Creation date: 2026-05-22, number of genomes: 144, number of BUSCOs: 1122)
# BUSCO was run in mode: proteins

	***** Results: *****

	C:89.8%[S:88.3%,D:1.4%],F:6.5%,M:3.7%,n:1122	   
	1007	Complete BUSCOs (C)			   
	991	Complete and single-copy BUSCOs (S)	   
	16	Complete and duplicated BUSCOs (D)	   
	73	Fragmented BUSCOs (F)			   
	42	Missing BUSCOs (M)			   
	1122	Total BUSCO groups searched		   

Dependencies and versions:
	hmmsearch: 3.4

```
