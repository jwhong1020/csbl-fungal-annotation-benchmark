# EviAnn Gene Prediction Summary

## Analysis overview

- **Sample:** NIBR_biolum
- **Pipeline:** EviAnn v2.0.5
- **Threads:** 16
- **Genome:** `NIBRbiolumScaffold.fa`
- **RNA-Seq evidence:** Paired-end RNA-Seq
- **Protein evidence:** NCBI Omphalotaceae protein sequences
- **Protein taxon:** Omphalotaceae, NCBI Taxonomy ID 72117
- **Logging options:** `--debug`, `--verbose`

## Input data

| Input | File |
|---|---|
| Genome assembly | `NIBRbiolumScaffold.fa` |
| RNA-Seq read 1 | `fastpQC_1.fastq.gz` |
| RNA-Seq read 2 | `fastpQC_2.fastq.gz` |
| RNA-Seq manifest | `reads.txt` |
| Protein evidence | `omphalotaceae_all_proteins.faa` |

## Methods

EviAnn v2.0.5 was used to annotate the NIBR_biolum genome assembly using RNA-Seq and protein evidence. 
Paired-end RNA-Seq reads were supplied through `reads.txt`. Protein sequences belonging to the family Omphalotaceae 
were downloaded from the NCBI Protein database using NCBI EDirect with taxonomy ID 72117 and supplied as external protein evidence.  
Followed the method from here : https://github.com/alekseyzimin/EviAnn_release  
EviAnn was run using 16 threads with debug and verbose logging enabled.

### RNA-Seq input format

```text
/path/to/fastpQC_1.fastq.gz /path/to/fastpQC_2.fastq.gz fastq
```

## Result
### 1. Gene prediction results
|Metric|	Result |
|-------|--------|
|Genes	|7,760|
|Transcripts|	11,644|
|Transcripts per gene|	1.50|
|Exons	| 98,735|
|CDS features	| 67,460|
|Exons per transcript |	8.48|
|Single-exon transcripts |	299 (2.57%) |
|Multi-exon transcripts |	11,345 (97.43%)|

EviAnn predicted 7,760 genes and 11,644 transcripts. The number of transcripts was approximately 1.5 times the number of genes, indicating that multiple transcript isoforms were predicted for some genes.  

Most predicted transcripts were multi-exonic.

### 2. CDS length statistics
|Metric	| Result |
|-------|--------|
|Transcripts with CDS |	11,644|
|Mean CDS length |	1,143.28 bp|
|Median CDS length |	988.50 bp|
|Minimum CDS length	| 33 bp|
|Maximum CDS length |	10,047 bp|

#### Short CDS counts
|CDS length|	Count|	Percentage|
|----------|-------|----------|
|< 90 bp|	18|	0.15%|
|< 150 bp	|50	|0.43%|
|< 300 bp	|574|	4.93%|

### 3. Gene span statistics
| Metric | Result |
| ---------------- | ----------- |
| Genes | 7,760 |
| Mean gene span | 2,631.34 bp |
| Median gene span | 2,191 bp |

Gene span represents the genomic region from the start to the end of each predicted gene, including both exons and introns.

The mean gene span was higher than the median, suggesting that a subset of relatively long gene models contributed to increasing the average value.

### 4.Representative protein dataset
To reduce redundancy caused by alternative transcript isoforms, the longest protein isoform was selected for each gene.

| Dataset | Protein sequences |
| ------------------------ | ----------------- |
| All isoforms | 11,644 |
| Longest isoform per gene | 7,760 |

The representative protein dataset contains one protein sequence per gene, corresponding to the longest isoform among predicted transcripts.

## BUSCO Assessment
Used fungi_odb 12 as BRAKER4 did.

| Dataset | Complete | Single-copy | Duplicated | Fragmented | Missing |
| ------------------------ | -------- | ----------- | ---------- | ---------- | ------- |
| All isoforms | 58.6% | 44.4% | 14.3% | 16.6% | 24.8% |
| Longest isoform per gene | 58.6% | 57.8% | 0.9% | 16.5% | 24.9% |

The overall complete BUSCO score remained unchanged at **58.6%** after selecting the longest isoform per gene.

However, duplicated BUSCOs decreased from **14.3% to 0.9%**, while single-copy BUSCOs increased from **44.4% to 57.8%**. This indicates that most duplicated BUSCO matches in the all-isoform dataset were caused by alternative transcript isoforms rather than independent gene duplications.

Therefore, the longest-isoform protein dataset is more appropriate for comparing BUSCO completeness across different annotation pipelines.

The longest-isoform dataset contained:

- **58.6% complete BUSCOs**
- **16.5% fragmented BUSCOs**
- **24.9% missing BUSCOs**

A substantial proportion of conserved fungal orthologs were fragmented or not detected in the EviAnn annotation. However, annotation quality should not be evaluated based on this result alone. It should be interpreted alongside genome assembly BUSCO scores and results from other annotation pipelines.  

The BUSCO score of the original assembly genome is higher than that of eviANN.
- Genome BUSCO: 98.8%
- BRAKER4 proteome BUSCO: 91.4%
- EviAnn proteome BUSCO: 58.6%

| Comparison         | Complete BUSCO difference |
| ------------------ | ------------------------: |
| Genome vs. EviAnn  |                    40.2%p |
| BRAKER4 vs. EviAnn |                    32.8%p |

EviAnn proteome showed substantially lower BUSCO completeness than both the genome assembly and BRAKER4 proteome. This suggests that many conserved fungal genes were missed or incompletely predicted by EviAnn rather than being absent from the assembly.

### Main output files
| File                                      | Description                                |
| ----------------------------------------- | ------------------------------------------ |
| `NIBRbiolumScaffold.fa.pseudo_label.gff`  | Final annotation after pseudogene-labeling |
| `NIBRbiolumScaffold.fa.gff`               | Gene annotation in GFF format              |
| `NIBRbiolumScaffold.fa.gtf`               | Gene annotation in GTF format              |
| `NIBRbiolumScaffold.fa.proteins.fasta`    | Predicted protein sequences                |
| `NIBRbiolumScaffold.fa.transcripts.fasta` | Predicted transcript sequences             |
| `eviann_omphalotaceae.log`                | EviAnn execution log                       |
