# BRAKER4 Gene Prediction Summary

## Analysis overview

* Sample: `NIBR_biolum`
* Pipeline: BRAKER4 v0.5.0-beta
* Mode: ETP/BRAKER3
* Evidence: RNA-Seq + protein
* BUSCO lineage: `fungi_odb12`
* Runtime: 4.4 h

## Methods

Genome repeats were soft-masked using RepeatModeler2, RepeatMasker, and Tandem Repeats Finder. RNA-Seq reads were aligned to the genome using HISAT2. GeneMark-ETP and AUGUSTUS were used for gene prediction, and the results were integrated using TSEBRA. Missing conserved genes were rescued using `best_by_compleasm`. UTRs were added using StringTie2, and the final annotation was converted to GFF3 format using AGAT.

## Gene prediction results

| Metric                  |         Result |
| ----------------------- | -------------: |
| Genes                   |         14,337 |
| Transcripts             |         16,804 |
| Transcripts per gene    |           1.17 |
| Exons per transcript    |           7.14 |
| Single-exon transcripts |   1,246 (7.4%) |
| Multi-exon transcripts  | 15,558 (92.6%) |
| Mean CDS length         |       1,238 bp |
| Median CDS length       |         993 bp |

GeneMark-ETP initially predicted 11,996 genes, while AUGUSTUS predicted 14,266 genes. After integration, BUSCO rescue, internal stop codon filtering, and CDS normalization, the final gene set contained 14,337 genes and 16,804 transcripts.

## Completeness assessment

| Dataset  | Tool      | Complete | Fragmented | Missing |
| -------- | --------- | -------: | ---------: | ------: |
| Genome   | BUSCO     |    98.8% |       0.2% |    1.0% |
| Genome   | compleasm |    99.2% |       0.5% |    0.4% |
| Proteome | BUSCO     |    91.4% |       5.2% |    3.5% |
| Proteome | compleasm |    92.0% |       5.0% |    3.0% |

The genome assembly showed high completeness, with approximately 99% of conserved fungal genes detected. Proteome completeness was lower at approximately 92%, indicating that some conserved genes present in the genome may not have been completely reconstructed in the predicted gene models.

Compleasm reported a high duplicated BUSCO proportion in the proteome. 
This should be re-evaluated using one representative protein isoform per gene because `braker.aa` may contain multiple transcript isoforms from the same locus.

## Main output files

| File                  | Description                         |
| --------------------- | ----------------------------------- |
| `braker.gff3.gz`      | Final structural annotation         |
| `braker.gtf.gz`       | Final gene prediction in GTF format |
| `braker.aa.gz`        | Predicted protein sequences         |
| `braker.codingseq.gz` | Predicted CDS sequences             |
| `braker_utr.gtf.gz`   | Gene models with UTRs               |
| `gene_support.tsv`    | Per-gene evidence support           |
| `hintsfile.gff.gz`    | RNA-Seq and protein evidence hints  |

## Conclusion

BRAKER4 predicted 14,337 genes from the `NIBR_biolum` genome. The genome assembly was highly complete, whereas the predicted proteome showed approximately 92% completeness. The BRAKER4 result will be compared with FunGAP and Helixer based on gene number, gene structure, evidence support, and BUSCO completeness.
