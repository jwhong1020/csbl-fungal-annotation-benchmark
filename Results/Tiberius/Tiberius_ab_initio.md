# Tiberius ab initio Gene Prediction Summary

## Analysis overview

| Item | Description |
| --- | --- |
| Pipeline | Tiberius |
| Prediction mode | Ab initio |
| Evidence | Genome sequence only |
| BUSCO lineage | fungi_odb12 |
| UTR prediction | Not included |
| Representative isoform selection | Not required |

Tiberius ab initio prediction does not provide a separate `gene` feature. Therefore, the number of `mRNA` features was used as the number of gene models. Each gene model contained one transcript.

## 1. Gene prediction results

| Metric | Result |
| --- | ---: |
| Gene models | 12,242 |
| Transcripts | 12,242 |
| Transcripts per gene | 1.00 |
| Exons | 79,365 |
| CDS features | 79,365 |
| Mean exons per transcript | 6.48 |
| Single-exon transcripts | 780 (6.37%) |
| Multi-exon transcripts | 11,462 (93.63%) |

Tiberius predicted 12,242 gene models, with one transcript per gene model. Most transcripts were multi-exonic, while single-exon transcripts accounted for 6.37% of the predictions.

## 2. CDS length statistics

| Metric | Result |
| --- | ---: |
| CDS models | 12,242 |
| Mean CDS length | 1,345.53 bp |
| Median CDS length | 1,119.00 bp |
| CDS shorter than 90 bp | 0 (0.00%) |
| CDS shorter than 150 bp | 0 (0.00%) |
| CDS shorter than 300 bp | 296 (2.42%) |

The mean CDS length was greater than the median, indicating that relatively long CDS models increased the mean value. CDS models shorter than 300 bp accounted for 2.42% of the total predictions.

## 3. Gene span statistics

| Metric | Result |
| --- | ---: |
| Gene models | 12,242 |
| Mean gene span | 1,688.62 bp |
| Median gene span | 1,426.00 bp |

Because Tiberius does not provide a separate `gene` feature, the genomic coordinates of each `mRNA` feature were 
used as the gene model span. The mean gene span was greater than the median, indicating the presence of 
relatively long gene models.

## 4. BUSCO completeness

| BUSCO category | Count | Percentage |
| --- | ---: | ---: |
| Complete BUSCOs | 980 | 87.3% |
| Complete and single-copy BUSCOs | 964 | 85.9% |
| Complete and duplicated BUSCOs | 16 | 1.4% |
| Fragmented BUSCOs | 76 | 6.8% |
| Missing BUSCOs | 66 | 5.9% |
| Total BUSCO groups | 1,122 | 100.0% |

BUSCO analysis using the `fungi_odb12` lineage showed 87.3% complete BUSCOs. 
Fragmented and missing BUSCOs accounted for 6.8% and 5.9%, respectively. 
Thus, Tiberius ab initio recovered most conserved fungal genes, but some conserved genes remained incomplete or undetected.

## 5. Completeness comparison

| Gene prediction model | Complete BUSCO |
| --- | ---: |
| ANNEVO | 89.8% |
| Helixer | 88.1% |
| Tiberius ab initio | 87.3% |

Tiberius ab initio showed the lowest complete BUSCO score among the three models. 
Its completeness was 2.5 percentage points lower than ANNEVO and 0.8 percentage points lower than Helixer. However, the differences were relatively small, and BUSCO completeness alone cannot determine the structural accuracy of individual gene models.

## Overall summary

Tiberius ab initio predicted 12,242 gene models, with one transcript per gene model. 
The predictions contained an average of 6.48 exons per transcript, and 93.63% of transcripts were multi-exonic. 
The mean CDS length was 1,345.53 bp, and 2.42% of CDS models were shorter than 300 bp. 
BUSCO completeness was 87.3%, which was slightly lower than ANNEVO and Helixer.
