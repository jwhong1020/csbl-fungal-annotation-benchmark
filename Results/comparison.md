# Comparison among models
## Before polishing

| Model                  | Evidence                        |  Genes | Transcripts | Tx/gene | Exons/tx |    Mean CDS | Complete BUSCO | Fragmented |   Missing |
| ---------------------- | ------------------------------- | -----: | ----------: | ------: | -------: | ----------: | -------------: | ---------: | --------: |
| **BRAKER4**            | RNA-seq + protein               | 14,337 |      16,804 |    1.17 |     7.14 |    1,238 bp |      **91.4%** |       5.2% |  **3.5%** |
| **ANNEVO**             | 사전학습 모델                    |    13,108 |         13,108 |       1.00 |       6.5 | 1,278bp |          89.8% |       6.5% |      3.7% |
| **Helixer**            | 사전학습 모델                         | 15,304 |      15,304 |    1.00 |  약 6.45* | 1,239bp |          88.1% |       7.8% |      4.0% |
| **Tiberius ab initio** | genome only                     | 12,242 |      12,242 |    1.00 |     6.48 | 1,345.53 bp |          87.3% |       6.8% |      5.9% |
| **Tiberius evidence**  | RNA-seq + Fungi.fa              | 11,281 |      15,670 |    1.39 |     7.46 | 1,325.07 bp |         87.3%† |       6.8% |      5.9% |
| **EviAnn**             | RNA-seq + Omphalotaceae protein |  7,760 |      11,644 |    1.50 |     8.48 | 1,143.28 bp |      **58.6%** |  **16.5%** | **24.9%** |

# Genome Annotation Model Evaluation - After polishing

## Overview

Genome annotation quality was evaluated using two complementary approaches:

1. **BUSCO analysis**
   - Evaluates the recovery of conserved single-copy orthologs.
   - Used here to estimate the overall completeness of each predicted proteome.

2. **DIAMOND BLASTP analysis**
   - Compares predicted proteins against four known fungal bioluminescence-related proteins.
   - Used to determine whether biologically important target genes were recovered as complete protein models.

The following annotation models were compared:

- BRAKER4
- ANNEVO
- Tiberius ab initio
- Tiberius evidence
- Helixer
- EviAnn

---

## 1. BUSCO Completeness

BUSCO analysis was performed using the `fungi_odb12` lineage dataset.

| Annotation | Complete | Single-copy | Duplicated | Fragmented | Missing |
|---|---:|---:|---:|---:|---:|
| **BRAKER4** | **99.0%** | **97.2%** | 1.8% | **0.5%** | **0.4%** |
| **ANNEVO** | 98.3% | 96.7% | 1.6% | 1.2% | **0.4%** |
| Tiberius ab initio | 96.9% | 95.5% | **1.4%** | 1.5% | 1.6% |
| Tiberius evidence | 96.9% | 95.5% | **1.4%** | 1.5% | 1.6% |
| Helixer | 96.7% | 95.2% | 1.5% | 2.0% | 1.3% |
| EviAnn | 87.0% | 64.0% | 23.0% | 2.7% | 10.3% |

### Interpretation

- **BRAKER4 showed the highest BUSCO completeness**, with 99.0% complete BUSCOs.
- BRAKER4 also showed a low fragmented proportion of 0.5% and a low missing proportion of 0.4%.
- **ANNEVO ranked second**, with 98.3% complete BUSCOs and the same missing proportion as BRAKER4.
- Tiberius ab initio and Tiberius evidence produced identical BUSCO results.
- Helixer also showed high completeness, although its fragmented BUSCO proportion was slightly higher.
- EviAnn showed the lowest completeness and substantially higher duplicated and missing BUSCO proportions.

Based only on conserved gene recovery, the overall ranking was:

1. BRAKER4
2. ANNEVO
3. Tiberius ab initio / Tiberius evidence
4. Helixer
5. EviAnn

> BUSCO completeness measures the recovery of conserved orthologs. A high BUSCO score does not directly demonstrate that every exon, intron, translation start site, or translation stop site was predicted correctly.

---

## 2. BRAKER4 Pipeline Summary

BRAKER4 was run in **ETP/BRAKER3 mode**, integrating both RNA-seq and protein evidence.

| Item | BRAKER4 setting |
|---|---|
| Pipeline | BRAKER4 v0.5.0-beta |
| Mode | ETP/BRAKER3 |
| RNA-seq evidence | Included |
| Protein evidence | Included |
| Repeat masking | RepeatModeler2, RepeatMasker, and Tandem Repeats Finder |
| RNA-seq aligner | HISAT2 |
| BUSCO lineage | `fungi_odb12` |
| Total wall-clock time | 3.8 h |
| Total CPU time | 32.2 h |

### BRAKER4 BUSCO results

| Dataset | Complete | Single-copy | Duplicated | Fragmented | Missing |
|---|---:|---:|---:|---:|---:|
| Genome assembly | 99.1% | 97.6% | 1.5% | 0.2% | 0.7% |
| Predicted proteome | 99.0% | 97.2% | 1.8% | 0.5% | 0.4% |

The similarity between the genome assembly BUSCO score and the BRAKER4 proteome BUSCO score indicates that BRAKER4 recovered most of the conserved genes detectable in the assembly.

However, this comparison does not by itself verify the accuracy of individual gene structures.

---

## 3. Bioluminescence-Related Protein Recovery

Four proteins associated with the fungal bioluminescence pathway were used as DIAMOND BLASTP queries.

| Gene | UniProt query | Function |
|---|---|---|
| **LUZ** | `A0A3G9JYH7.1` | Luciferase |
| **CPH** | `A0A3G9JYJ6.1` | Caffeoylpyruvate hydrolase |
| **HispS** | `A0A3G9K3K9.1` | Hispidin synthase |
| **H3H** | `A0A3G9K5C8.1` | Hispidin-3-hydroxylase |

### Hit classification

| Classification | Interpretation |
|---|---|
| **Strong** | Most of both the query and target proteins were aligned |
| **Partial** | Significant homology was detected, but a large part of the query or target was not aligned |
| **Weak** | Only a limited region was aligned or the predicted protein was strongly truncated |

---

## 4. Summary of DIAMOND BLASTP Results

| Annotation | LUZ | CPH | HispS | H3H | Strong hits |
|---|---|---|---|---|---:|
| BRAKER4 | Not evaluated | Not evaluated | Not evaluated | Not evaluated | N/A |
| **ANNEVO** | Strong | Strong | Strong | Strong | **4/4** |
| **Tiberius ab initio** | Strong | Strong | Strong | Strong | **4/4** |
| **Tiberius evidence** | Strong | Strong | Strong | Strong | **4/4** |
| Helixer | Strong | Strong | Partial | Strong | 3/4 |
| EviAnn | Strong | Strong | Partial | Weak | 2/4 |

### Interpretation

- ANNEVO, Tiberius ab initio, and Tiberius evidence recovered all four bioluminescence-related proteins as strong hits.
- Helixer recovered LUZ, CPH, and H3H as strong hits, but its HispS prediction covered only approximately half of the reference protein.
- EviAnn recovered LUZ and CPH as strong hits, whereas HispS was partial and H3H was weak.
- BRAKER4 could not be included in this comparison because a corresponding DIAMOND BLASTP result was not available in the current dataset.

---

## 5. LUZ Comparison

| Annotation | Identity | Query coverage | Target coverage | E-value | Classification |
|---|---:|---:|---:|---:|---|
| ANNEVO | 84.80% | 90.64% | 96.83% | 2.00e-153 | Strong |
| Tiberius ab initio | 84.80% | 90.64% | 96.83% | 2.03e-153 | Strong |
| Tiberius evidence | 84.80% | 90.64% | 96.83% | 2.03e-153 | Strong |
| EviAnn | 84.80% | 90.64% | 96.83% | 2.03e-153 | Strong |
| Helixer | 84.80% | 90.64% | 91.73% | 3.70e-153 | Strong |

### Interpretation

- All evaluated models recovered LUZ as a strong hit.
- Identity and query coverage were identical among the models.
- Helixer had a slightly lower target coverage than the other models.
- The differences among models were relatively small for LUZ.

---

## 6. CPH Comparison

| Annotation | Identity | Query coverage | Target coverage | E-value | Classification |
|---|---:|---:|---:|---:|---|
| ANNEVO | 63.00% | 96.89% | 98.01% | 2.14e-123 | Strong |
| Tiberius ab initio | 63.00% | 96.89% | 98.01% | 2.17e-123 | Strong |
| Tiberius evidence | 63.00% | 96.89% | 98.01% | 2.17e-123 | Strong |
| Helixer | 63.00% | 96.89% | 86.76% | 9.03e-123 | Strong |
| EviAnn | 63.00% | 96.89% | 86.76% | 8.17e-123 | Strong |

### Interpretation

- All evaluated models recovered CPH as a strong hit.
- Query coverage was 96.89% for every model.
- ANNEVO and both Tiberius models showed higher target coverage than Helixer and EviAnn.
- The longer predicted proteins in Helixer and EviAnn contained regions that were not aligned to the reference query.

---

## 7. HispS Comparison

| Annotation | Predicted protein length | Identity | Query coverage | Target coverage | Bitscore | Classification |
|---|---:|---:|---:|---:|---:|---|
| **ANNEVO** | 1,692 aa | 76.00% | **99.82%** | **99.41%** | **2511.0** | **Strong** |
| **Tiberius ab initio** | 1,692 aa | 76.00% | **99.82%** | **99.41%** | **2511.0** | **Strong** |
| **Tiberius evidence** | 1,692 aa | 76.00% | **99.82%** | **99.41%** | **2511.0** | **Strong** |
| Helixer | 851 aa | 76.00% | 48.57% | 95.18% | 1245.0 | Partial |
| EviAnn | 2,126 aa | 29.70% | 48.45% | 38.43% | 283.0 | Partial |

### Interpretation

- HispS produced the clearest difference among the annotation models.
- ANNEVO and both Tiberius models predicted proteins of 1,692 amino acids.
- Their query and target coverage values were both approximately 99%, indicating nearly complete recovery of the reference HispS protein.
- The Helixer protein was 851 amino acids long and covered only 48.57% of the reference query.
- Although most of the Helixer target protein aligned to HispS, only approximately half of the reference protein was recovered.
- The EviAnn candidate was longer than the reference protein but had low identity and low coverage.
- The EviAnn result may represent an incorrectly merged gene model, a distantly homologous protein, or another structurally inconsistent prediction.
- Gene-coordinate and domain-level analyses are required to distinguish among these possibilities.

---

## 8. H3H Comparison

| Annotation | Predicted protein length | Identity | Query coverage | Target coverage | Bitscore | Classification |
|---|---:|---:|---:|---:|---:|---|
| **ANNEVO** | 455 aa | **66.20%** | 99.05% | 98.68% | **550.0** | Strong |
| Helixer | 457 aa | 65.50% | 99.05% | 98.69% | 544.0 | Strong |
| Tiberius ab initio | 440 aa | 61.70% | 99.05% | 98.64% | 502.0 | Strong |
| Tiberius evidence | 440 aa | 61.70% | 99.05% | 98.64% | 502.0 | Strong |
| EviAnn | 127 aa | 72.60% | 29.38% | 97.64% | 181.0 | Weak |

### Interpretation

- ANNEVO, Helixer, and both Tiberius models recovered nearly the full H3H reference protein.
- Their query and target coverage values were approximately 99%.
- ANNEVO had the highest identity and bitscore.
- The EviAnn protein was only 127 amino acids long compared with the 422-amino-acid reference query.
- Most of the short EviAnn protein aligned to H3H, but it represented only 29.38% of the full reference protein.
- The EviAnn H3H prediction was therefore classified as weak and strongly truncated.

---

## 9. Effect of Evidence on Tiberius

| Evaluation metric | Tiberius ab initio | Tiberius evidence |
|---|---:|---:|
| Complete BUSCO | 96.9% | 96.9% |
| Single-copy BUSCO | 95.5% | 95.5% |
| Duplicated BUSCO | 1.4% | 1.4% |
| Fragmented BUSCO | 1.5% | 1.5% |
| Missing BUSCO | 1.6% | 1.6% |
| Strong bioluminescence-related hits | 4/4 | 4/4 |

### Interpretation

- No difference was detected between the Tiberius ab initio and evidence-supported models using the current evaluation metrics.
- The BUSCO results were identical.
- The best DIAMOND hits for the four bioluminescence-related proteins also had identical lengths, identities, coverage values, E-values, and bitscores.
- This does not demonstrate that the complete annotation files are identical.
- Differences may still exist in other genes, transcript isoforms, untranslated regions, or exon–intron structures.

---

## 10. Integrated Evaluation

| Annotation | BUSCO completeness | Bioluminescence gene recovery | Main observation |
|---|---:|---:|---|
| **BRAKER4** | **99.0%** | Not evaluated | Highest conserved-gene completeness; RNA-seq and protein evidence integrated |
| **ANNEVO** | 98.3% | **4/4 strong** | Highest evaluated performance across both BUSCO and target-gene recovery |
| **Tiberius ab initio** | 96.9% | **4/4 strong** | Complete recovery of all four target proteins |
| **Tiberius evidence** | 96.9% | **4/4 strong** | Same evaluated results as the ab initio model |
| Helixer | 96.7% | 3/4 strong | HispS was predicted as an approximately half-length protein |
| EviAnn | 87.0% | 2/4 strong | High BUSCO duplication and incomplete HispS and H3H predictions |

---

## 11. Overall Conclusion

BRAKER4 produced the highest BUSCO completeness, recovering 99.0% of the conserved fungal orthologs in its predicted proteome.

ANNEVO showed the second-highest BUSCO completeness at 98.3% and recovered all four bioluminescence-related proteins as strong and nearly complete matches.

Both Tiberius models also recovered all four target proteins as strong hits, although their BUSCO completeness was lower than that of BRAKER4 and ANNEVO.

Helixer showed high overall BUSCO completeness, but its HispS model was approximately half the expected protein length.

EviAnn showed the lowest BUSCO completeness, a high duplicated BUSCO proportion, and incomplete predictions for HispS and H3H.

Based on the currently available results:

- **BRAKER4 had the highest overall conserved-gene completeness.**
- **ANNEVO had the strongest combined evidence from BUSCO and target-gene recovery.**
- **Tiberius was comparable to ANNEVO for the four bioluminescence-related proteins.**
- **Helixer produced a likely truncated HispS model.**
- **EviAnn showed the weakest overall performance among the evaluated models.**

A definitive annotation model should not be selected from BUSCO and four target proteins alone.

Additional evaluation should include:

- RNA-seq transcript support
- Splice-junction support
- Exon and intron boundary accuracy
- Gene splitting and gene fusion
- Protein-domain completeness
- Abnormally short or long gene models
- Transcript count and isoform inflation
- Agreement with known gene structures
- Manual inspection of the bioluminescence gene loci
