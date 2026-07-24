# ANNEVO Gene Prediction Summary

## Analysis overview
- **Sample:** NIBR_biolum
- **Pipeline:** EviAnn v2.0.5
- **Threads:** 16
- **Genome:** `assembly.fa`
- **RNA-Seq evidence:** Paired-end RNA-Seq

## Input data
| Input | File |
|---|---|
| Genome assembly | `assembly.fasta` |
| model | `ANNEVO_Fungi` |

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
