# Bioluminescence Gene Detection across Genome Annotation Results

## Overview

Six genome annotation results were evaluated to determine whether four known bioluminescence-related proteins from *Neonothopanus nambi* were detected in each predicted proteome.

The evaluated annotation results were:

1. BRAKER4
2. EviAnn
3. Tiberius evidence-based
4. Tiberius ab initio
5. Helixer
6. ANNEVO

The four query proteins were:

- LUZ: luciferase
- CPH: caffeoylpyruvate hydrolase
- HIPS: hispidin synthase
- H3H: hispidin-3-hydroxylase

Protein similarity was evaluated using DIAMOND `blastp`.

---

## Interpretation of coverage

### Query coverage

Query coverage represents the proportion of the reference protein covered by the alignment.
A high query coverage indicates that most of the reference protein is represented in the predicted protein.

### Target coverage

Target coverage represents the proportion of the predicted protein covered by the alignment.
A high target coverage indicates that most of the predicted protein corresponds to the reference protein.

### Combined interpretation

| Query coverage | Target coverage | Interpretation |
|---:|---:|---|
| High | High | Nearly full-length and clean protein prediction |
| Low | High | Predicted protein corresponds to only part of the reference protein |
| High | Low | Reference protein is largely included, but the prediction contains additional non-homologous regions |
| Low | Low | Partial similarity or potentially inaccurate gene model |

Both query coverage and target coverage should be considered together.

---

# Results

## 1. Overall detection

All four bioluminescence-related proteins were detected in all six annotation results.

| Gene | BRAKER4 | EviAnn | Tiberius evidence | Tiberius ab initio | Helixer | ANNEVO |
|---|---|---|---|---|---|---|
| LUZ | Detected | Detected | Detected | Detected | Detected | Detected |
| CPH | Detected | Detected | Detected | Detected | Detected | Detected |
| HIPS | Detected | Detected | Detected | Detected | Detected | Detected |
| H3H | Detected | Detected | Detected | Detected | Detected | Detected |

Detection alone does not indicate that the full gene structure was correctly predicted. 
Therefore, identity, query coverage, target coverage, target protein length, and bitscore were compared.

---

## 2. LUZ

| Annotation | Target | Identity (%) | Query coverage (%) | Target coverage (%) | Bitscore |
|---|---|---:|---:|---:|---:|
| BRAKER4 | g180.t2 | 84.5 | 86.5 | 93.9 | 409 |
| EviAnn | LOC_00003087-mRNA-1 | 84.0 | 88.76 | 94.44 | 417 |
| Tiberius evidence | gene_000163.t1 | 84.0 | 88.76 | 94.44 | 417 |
| Tiberius ab initio | g163\|g166.t1 | 84.0 | 88.76 | 94.44 | 417 |
| Helixer | _Scaffold1_001374.1 | 84.0 | 88.76 | 89.47 | 417 |
| ANNEVO | Scaffold1-g740.t1 | 84.0 | 88.76 | 94.44 | 417 |

### Interpretation

- LUZ was detected in all six annotation results.
- EviAnn, Tiberius, Helixer, and ANNEVO showed nearly identical identity and query coverage.
- BRAKER4 also produced a strong LUZ hit, although its query coverage and bitscore were slightly lower than those of the other models.
- Overall, LUZ prediction was highly consistent across annotation tools.

---

## 3. CPH

| Annotation | Target | Identity (%) | Query coverage (%) | Target coverage (%) | Bitscore |
|---|---|---:|---:|---:|---:|
| BRAKER4 | g12231.t1 | 63.0 | 96.9 | 98.0 | 353 |
| EviAnn | LOC_00002011-mRNA-1 | 63.0 | 96.89 | 86.76 | 353 |
| Tiberius evidence | gene_010441.t1 | 63.0 | 96.89 | 86.76 | 353 |
| Tiberius ab initio | g618\|g10667.t1 | 63.0 | 96.89 | 86.76 | 353 |
| Helixer | _Scaffold6_000470.1 | 63.0 | 96.89 | 84.05 | 353 |
| ANNEVO | Scaffold6-g680.t1 | 63.0 | 96.89 | 86.76 | 353 |

### Interpretation

- CPH was detected in all six annotation results.
- Identity, query coverage, and bitscore were essentially identical across all annotation tools.
- BRAKER4 showed the highest target coverage, indicating that nearly the entire BRAKER4-predicted protein aligned to the reference CPH protein.
- Overall, CPH prediction was highly consistent among annotation tools.

---

## 4. HIPS

| Annotation | Target | Identity (%) | Query coverage (%) | Target coverage (%) | Bitscore |
|---|---|---:|---:|---:|---:|
| BRAKER4 | g238.t1 | 75.0 | 54.1 | 98.9 | 1,305 |
| EviAnn | LOC_00007553-mRNA-2 | 29.7 | 48.45 | 38.43 | 284 |
| Tiberius evidence | gene_000213.t1 | 72.5 | 54.11 | 98.87 | 1,248 |
| Tiberius ab initio | g213\|g216.t1 | 72.5 | 54.11 | 98.87 | 1,248 |
| Helixer | _Scaffold1_001342.1 | 73.5 | 48.45 | 96.42 | 1,145 |
| ANNEVO | Scaffold1-g772.t1 | 74.1 | 97.44 | 99.32 | 2,349 |

### Interpretation

- HIPS showed the largest difference among annotation tools.
- ANNEVO produced the most complete HIPS protein-level match, with 97.44% query coverage and 99.32% target coverage.
- BRAKER4 and both Tiberius models showed similar partial predictions, covering approximately 54% of the reference HIPS protein while nearly completely covering their predicted targets.
- Helixer also produced a partial model, with query coverage below 50%.
- EviAnn showed low identity and low coverage on both the query and target sides.
- The BRAKER4, Tiberius, and Helixer results are consistent with partial or split HIPS gene models, while the ANNEVO model is closest to a full-length prediction.

---

## 5. H3H

| Annotation | Target | Identity (%) | Query coverage (%) | Target coverage (%) | Bitscore |
|---|---|---:|---:|---:|---:|
| BRAKER4 | g181.t1 | 61.9 | 99.1 | 99.0 | 496 |
| EviAnn | LOC_00003088-mRNA-2 | 74.7 | 41.94 | 99.45 | 269 |
| Tiberius evidence | gene_000164.t1 | 67.2 | 75.36 | 99.72 | 442 |
| Tiberius ab initio | g164\|g167.t1 | 67.2 | 75.36 | 99.72 | 442 |
| Helixer | _Scaffold1_000108.1 | 64.1 | 99.05 | 99.08 | 528 |
| ANNEVO | Scaffold1-g95.t1 | 67.2 | 56.64 | 92.18 | 346 |

### Interpretation

- BRAKER4 and Helixer produced nearly full-length H3H predictions.
- Both showed approximately 99% query and target coverage.
- Helixer had a slightly higher identity and bitscore than BRAKER4.
- Tiberius produced an intermediate-length prediction, covering approximately 75% of the reference protein.
- EviAnn and ANNEVO produced partial H3H models despite relatively high target coverage.
- Overall, Helixer and BRAKER4 produced the strongest H3H predictions.

---

## 6. Summary comparison

| Annotation | LUZ | CPH | HIPS | H3H |
|---|---|---|---|---|
| BRAKER4 | Well detected | Well detected | Partial prediction | Nearly full-length |
| EviAnn | Well detected | Well detected | Low identity and low coverage | Partial prediction |
| Tiberius evidence | Well detected | Well detected | Partial prediction | Moderately complete |
| Tiberius ab initio | Well detected | Well detected | Partial prediction | Moderately complete |
| Helixer | Well detected | Well detected | Partial prediction | Most complete |
| ANNEVO | Well detected | Well detected | Most complete | Partial prediction |

---

## 7. Main findings

1. All four bioluminescence-related proteins were detected in all six predicted proteomes.
2. LUZ and CPH predictions were highly consistent among annotation tools.
3. ANNEVO produced the most complete HIPS model.
4. Helixer and BRAKER4 produced the most complete H3H models.
5. BRAKER4, Tiberius evidence-based, and Tiberius ab initio produced similar partial HIPS predictions.
6. Tiberius evidence-based and ab initio results were identical for all four evaluated proteins.
7. EviAnn produced partial or potentially inaccurate models for HIPS and H3H.
8. DIAMOND similarity alone cannot definitively establish gene-structure accuracy.
9. HIPS and H3H loci should be further inspected using genomic coordinates, exon structures, and RNA-seq alignment support.

---

## 8. Limitations

This analysis evaluates protein-level similarity to known *N. nambi* bioluminescence proteins.

A strong DIAMOND hit supports sequence homology but does not independently confirm:

- correct exon-intron boundaries
- correct translation start and stop sites
- correct isoform selection
- absence of gene fusion
- absence of gene splitting
- transcriptional support in the available RNA-seq data

In addition, the BRAKER4 results were obtained from a separately summarized result table. Direct comparison is most rigorous when all six proteomes are searched using the same DIAMOND version, parameters, query FASTA, and coverage calculation method.

---

## Conclusion

All six annotation results contained homolog candidates for LUZ, CPH, HIPS, and H3H.

LUZ and CPH were predicted consistently across annotation tools. HIPS and H3H showed substantial differences in predicted protein completeness.

ANNEVO produced the most complete HIPS model. Helixer and BRAKER4 produced nearly full-length H3H models, with Helixer showing the highest bitscore among the compared H3H candidates.

The results indicate that no single annotation tool produced the most complete model for every bioluminescence-related gene.
