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

| Metric                     |           Result |
| -------------------------- | ---------------: |
| Genes                      | To be calculated |
| Transcripts                |           11,644 |
| Distinct protein sequences |            9,422 |
| Long non-coding RNAs       |                0 |
| Pseudogene-labeled models  |                0 |
| Transcripts per gene       | To be calculated |
| Exons per transcript       | To be calculated |
| Single-exon transcripts    | To be calculated |

### 2. BUSCO

### Main output files
| File                                      | Description                                |
| ----------------------------------------- | ------------------------------------------ |
| `NIBRbiolumScaffold.fa.pseudo_label.gff`  | Final annotation after pseudogene-labeling |
| `NIBRbiolumScaffold.fa.gff`               | Gene annotation in GFF format              |
| `NIBRbiolumScaffold.fa.gtf`               | Gene annotation in GTF format              |
| `NIBRbiolumScaffold.fa.proteins.fasta`    | Predicted protein sequences                |
| `NIBRbiolumScaffold.fa.transcripts.fasta` | Predicted transcript sequences             |
| `eviann_omphalotaceae.log`                | EviAnn execution log                       |
