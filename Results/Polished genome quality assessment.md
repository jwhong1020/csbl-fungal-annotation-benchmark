# Polished Genome Quality Assessment

## Analysis overview

- **Genome:** *Omphalotus guepiniiformis* genome assembly
- **Polishing tool:** Polypolish
- **Polishing reads:** SRR21798704 paired-end Illumina reads
- **Assessment methods:** QUAST, BUSCO, and Illumina read mapping
- **BUSCO version:** 6.1.0
- **BUSCO lineage:** `fungi_odb12`
- **BUSCO dataset size:** 1,122 ortholog groups
- **BUSCO mode:** Genome mode
- **BUSCO gene predictor:** miniprot

## Input assemblies

| Assembly | File |
|---|---|
| Before polishing | `NIBRbiolumScaffold.fa` |
| After polishing | `NIBRbiolumScaffold.polypolish.fa` |

---

## 1. Assembly continuity

Assembly continuity and basic sequence statistics were evaluated using QUAST.

| Metric | Before polishing | After polishing | Change |
|---|---:|---:|---:|
| Number of contigs | 63 | 63 | 0 |
| Contigs ≥ 1 kb | 61 | 61 | 0 |
| Contigs ≥ 5 kb | 52 | 52 | 0 |
| Contigs ≥ 10 kb | 51 | 51 | 0 |
| Contigs ≥ 25 kb | 49 | 49 | 0 |
| Contigs ≥ 50 kb | 46 | 46 | 0 |
| Total length | 38,395,153 bp | 38,413,268 bp | +18,115 bp |
| Largest contig | 3,609,272 bp | 3,611,193 bp | +1,921 bp |
| N50 | 1,938,897 bp | 1,939,613 bp | +716 bp |
| N90 | 373,606 bp | 373,746 bp | +140 bp |
| auN | 1,758,563.5 | 1,759,495.0 | +931.5 |
| L50 | 8 | 8 | 0 |
| L90 | 27 | 27 | 0 |
| GC content | 46.78% | 46.80% | +0.02 percentage points |
| Ns per 100 kbp | 1,652.74 | 1,651.94 | −0.80 |

The total assembly length increased by 18,115 bp after polishing. The number of contigs, L50, and L90 remained unchanged, while N50 increased by only 716 bp.

These limited changes in continuity metrics are expected because Polypolish corrects nucleotide substitutions and short insertion or deletion errors within existing sequences rather than joining contigs or scaffolds.

Therefore, the nearly unchanged contig number and N50 do not indicate polishing failure. Instead, they show that the overall assembly structure was retained during sequence-level correction.

The number of ambiguous bases also decreased slightly from 1,652.74 to 1,651.94 Ns per 100 kbp.

---

## 2. Illumina read mapping

The paired-end Illumina reads used for polishing were aligned independently to the assemblies before and after polishing.

| Metric | Before polishing | After polishing | Change |
|---|---:|---:|---:|
| Mapped reads | 34,933,145 | 35,111,170 | +178,025 |
| Mapping rate | 85.70% | 86.12% | +0.42 percentage points |
| Properly paired reads | 31,673,866 | 31,920,238 | +246,372 |
| Covered bases | 35,872,723 bp | 36,006,936 bp | +134,213 bp |
| Genome coverage | 93.4303% | 93.7357% | +0.3054 percentage points |
| Mean depth | 133.6449× | 134.5611× | +0.9162× |

The mapping rate increased from 85.70% before polishing to 86.12% after polishing. This represents an increase of 0.42 percentage points and 178,025 additional mapped reads.

The number of properly paired reads increased by 246,372. Genome coverage increased from 93.4303% to 93.7357%, and the mean sequencing depth increased from 133.6449× to 134.5611×.

These results indicate that the polished assembly has improved agreement with the Illumina sequencing reads.

However, because the same reads were used both for polishing and for this mapping assessment, the mapping results are not an independent validation. They primarily demonstrate improved consistency between the polished genome and the read dataset used by Polypolish.

---

## 3. BUSCO completeness

Genome completeness was assessed using 1,122 conserved fungal ortholog groups from the `fungi_odb12` dataset.

| BUSCO category | Before polishing | After polishing | Change |
|---|---:|---:|---:|
| Complete BUSCOs | 1,109 (98.8%) | 1,112 (99.1%) | +3 (+0.3 percentage points) |
| Complete single-copy BUSCOs | 1,095 (97.6%) | 1,100 (98.0%) | +5 (+0.4 percentage points) |
| Complete duplicated BUSCOs | 14 (1.2%) | 12 (1.1%) | −2 (−0.1 percentage points) |
| Fragmented BUSCOs | 2 (0.2%) | 2 (0.2%) | 0 |
| Missing BUSCOs | 11 (1.0%) | 8 (0.7%) | −3 (−0.3 percentage points) |
| Complete BUSCOs with internal stop codons | 285 | 296 | +11 |

### BUSCO result before polishing

```text
C:98.8%[S:97.6%,D:1.2%],F:0.2%,M:1.0%,n:1122
```

### BUSCO result after polishing

```text
C:99.1%[S:98.0%,D:1.1%],F:0.2%,M:0.7%,n:1122
```

The percentage of complete BUSCOs increased from 98.8% to 99.1%. The number of complete BUSCOs increased from 1,109 to 1,112.

Complete single-copy BUSCOs increased from 1,095 to 1,100, whereas duplicated BUSCOs decreased from 14 to 12. The number of missing BUSCOs decreased from 11 to 8.

These results indicate a modest improvement in conserved gene completeness after polishing.

The fragmented BUSCO count remained unchanged at two, suggesting that polishing recovered several previously missing BUSCOs but did not resolve the two remaining fragmented gene regions.

---

## 4. Internal stop codons

BUSCO reported internal stop codons in a substantial number of complete gene predictions.

| Assembly | Complete BUSCOs with internal stop codons |
|---|---:|
| Before polishing | 285 |
| After polishing | 296 |
| Change | +11 |

The number of complete BUSCOs containing internal stop codons increased from 285 to 296 after polishing.

Therefore, the increase in overall BUSCO completeness does not demonstrate that all coding-region errors or frameshifts were corrected.

In genome mode, BUSCO uses miniprot to predict conserved gene structures directly from the genome assembly. Internal stop codons may therefore result from several possible sources:

- residual substitution or insertion/deletion errors in the genome assembly
- frameshifts within coding regions
- inaccurate exon–intron boundary prediction by miniprot
- incomplete or complex gene structures
- differences between BUSCO-predicted models and the final genome annotation
- sequence changes introduced or retained during polishing

The BUSCO summary alone cannot distinguish among these possibilities.

The affected BUSCO gene models should therefore be compared with the final structural annotation and predicted protein sequences before concluding that the internal stop codons represent genuine assembly errors.

---

## 5. Overall assessment

The following metrics improved after polishing:

- Mapping rate increased from 85.70% to 86.12%.
- The number of properly paired reads increased by 246,372.
- Genome coverage increased from 93.4303% to 93.7357%.
- Mean sequencing depth increased from 133.6449× to 134.5611×.
- Complete BUSCOs increased from 98.8% to 99.1%.
- Complete single-copy BUSCOs increased from 97.6% to 98.0%.
- Missing BUSCOs decreased from 1.0% to 0.7%.
- The number of Ns per 100 kbp decreased slightly.

The overall assembly structure remained stable. The number of contigs, L50, and L90 did not change, and N50 increased by only 716 bp.

This pattern is consistent with the intended function of Polypolish, which improves nucleotide-level consensus accuracy without substantially altering assembly continuity.

The simultaneous improvements in read mapping, genome coverage, and BUSCO completeness indicate that the polishing process was generally successful.

However, the increase in complete BUSCOs containing internal stop codons from 285 to 296 requires further investigation. Coding-region integrity should be evaluated using the final gene annotation, translated protein sequences, and, where available, RNA-seq evidence.

---

## Conclusion

The polished genome retained the overall structure of the original assembly while showing modest improvements in Illumina read alignment and conserved gene completeness.

The read mapping rate increased from 85.70% to 86.12%, genome coverage increased from 93.43% to 93.74%, and complete BUSCOs increased from 98.8% to 99.1%. Missing BUSCOs decreased from 11 to 8.

These results indicate that Polypolish improved the consistency of the genome assembly with the Illumina reads and slightly improved the recovery of conserved fungal genes without substantially changing assembly continuity.

Nevertheless, the number of complete BUSCOs containing internal stop codons increased from 285 to 296. Additional validation of coding regions is therefore required before the polished assembly is used as the final reference for structural genome annotation.

