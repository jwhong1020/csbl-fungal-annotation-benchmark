# EviAnn
Follow instructions below:
https://github.com/alekseyzimin/EviAnn_release

## Workflow

```text
Environment setup
    ↓
EviAnn installation
    ↓
Dependency installation
    ↓
Input file preparation
    ↓
Protein evidence download
    ↓
EviAnn gene prediction
    ↓
Annotation statistics
    ↓
Longest isoform selection
    ↓
BUSCO assessment
```

## 1. Installation

### 1.1. Create and activate a Conda environment

```bash
conda create -n eviann
conda activate eviann
```

### 1.2. Create a tools directory

```bash
mkdir -p tools
cd tools
```

### 1.3. Download EviAnn

```bash
wget https://github.com/alekseyzimin/EviAnn_release/releases/download/v2.0.5/EviAnn-2.0.5.tar.gz
ls -lh EviAnn-2.0.5.tar.gz
```

### 1.4. Extract and install EviAnn

```bash
tar xvzf EviAnn-2.0.5.tar.gz
cd EviAnn-2.0.5
./install.sh 2>&1 | tee install.log
```

### 1.5. Add EviAnn to `PATH` (optional)

```bash
echo 'export PATH="$HOME/tools/EviAnn-2.0.5/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Check the installation:

```bash
eviann.sh --help
```

### 1.6. Install dependencies

```bash
conda install -c bioconda -c conda-forge \
  minimap2 \
  hisat2
```

Check the installed programs:

```bash
which minimap2
which hisat2
which hisat2-build
```

## 2. Input preparation

### 2.1. Define project paths

Run the following commands from the project directory containing the genome and RNA-seq files.

```bash
PROJECT_DIR=$(pwd)
GENOME="$PROJECT_DIR/NIBRbiolumScaffold.fa"
READ1="$PROJECT_DIR/fastpQC_1.fastq.gz"
READ2="$PROJECT_DIR/fastpQC_2.fastq.gz"
ANALYSIS_DIR="$PROJECT_DIR/analysis/eviann"
```

### 2.2. Create the analysis directory

```bash
mkdir -p "$ANALYSIS_DIR"
cd "$ANALYSIS_DIR"
```

### 2.3. Create `reads.txt`

```bash
printf '%s\n' \
  "$READ1 $READ2 fastq" \
  > reads.txt
```

## 3. Protein evidence preparation

Protein sequences from species belonging to the family Omphalotaceae were downloaded from the NCBI Protein database and used as protein evidence.

Reference:

https://github.com/alekseyzimin/EviAnn_release#downloading-protein-evidence-from-ncbi

### 3.1. Create an input directory

```bash
mkdir -p "$ANALYSIS_DIR/input"
cd "$ANALYSIS_DIR/input"
```

### 3.2. Install NCBI EDirect

```bash
cd "$HOME"
sh -c "$(wget -q https://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh -O -)"
export PATH="$HOME/edirect:$PATH"
```

### 3.3. Check the number of available protein records

```bash
esearch \
  -db protein \
  -query 'txid72117[Organism:exp]' \
  | xtract -pattern ENTREZ_DIRECT -element Count
```

### 3.4. Download protein evidence

```bash
cd "$ANALYSIS_DIR/input"

export PATH="$HOME/edirect:$PATH"
set -o pipefail

esearch \
  -db protein \
  -query 'txid72117[Organism:exp]' \
  | efetch -format fasta \
  > omphalotaceae_all_proteins.faa
```

### 3.5. Check download progress

```bash
grep -c '^>' "$ANALYSIS_DIR/input/omphalotaceae_all_proteins.faa"
```

## 4. Run EviAnn

```bash
cd "$ANALYSIS_DIR"

PROTEIN_EVIDENCE="$ANALYSIS_DIR/input/omphalotaceae_all_proteins.faa"

eviann.sh \
  -t 16 \
  -g "$GENOME" \
  -r "$ANALYSIS_DIR/reads.txt" \
  -p "$PROTEIN_EVIDENCE" \
  --debug \
  --verbose \
  2>&1 | tee eviann_omphalotaceae.log
```

## 5. Annotation summary workflow

### 5.1. Define output files

```bash
cd "$ANALYSIS_DIR"

GFF=NIBRbiolumScaffold.fa.pseudo_label.gff
PROTEIN=NIBRbiolumScaffold.fa.proteins.fasta
TRANSCRIPT=NIBRbiolumScaffold.fa.transcripts.fasta

mkdir -p summary
```

### 5.2. Count feature types

```bash
awk -F '\t' '
!/^#/ && NF>=9 {
    count[$3]++
}
END {
    for (f in count)
        print f, count[f]
}' "$GFF" | sort -k2,2nr
```

### 5.3. Count genes, transcripts, exons, and CDS features

```bash
awk -F '\t' '
!/^#/ && NF>=9 {
    if ($3=="gene") gene++
    else if ($3=="mRNA") mrna++
    else if ($3=="exon") exon++
    else if ($3=="CDS") cds++
}
END {
    print "Genes\t" gene+0
    print "Transcripts\t" mrna+0
    print "Exons\t" exon+0
    print "CDS features\t" cds+0
}' "$GFF" | tee summary/basic_counts.tsv
```

### 5.4. Calculate exon statistics

```bash
awk -F '\t' '
!/^#/ && $3=="exon" {
    attr=$9

    sub(/^.*Parent=/, "", attr)
    sub(/;.*/, "", attr)

    n=split(attr, parent, ",")

    for (i=1; i<=n; i++) {
        if (parent[i] != "")
            exon_count[parent[i]]++
    }
}
END {
    total=0
    single=0
    multi=0
    transcripts=0

    for (id in exon_count) {
        transcripts++
        total += exon_count[id]

        if (exon_count[id] == 1)
            single++
        else
            multi++
    }

    printf "Transcripts_with_exons\t%d\n", transcripts

    if (transcripts > 0) {
        printf "Exons_per_transcript\t%.2f\n", total/transcripts
        printf "Single_exon_transcripts\t%d\n", single
        printf "Single_exon_percent\t%.2f\n", single/transcripts*100
        printf "Multi_exon_transcripts\t%d\n", multi
        printf "Multi_exon_percent\t%.2f\n", multi/transcripts*100
    }
}' "$GFF" | tee summary/exon_statistics.tsv
```

### 5.5. Calculate transcripts per gene

```bash
awk -F '\t' '
!/^#/ && $3=="gene" {g++}
!/^#/ && $3=="mRNA" {t++}
END {
    printf "Genes\t%d\n", g
    printf "Transcripts\t%d\n", t

    if (g > 0)
        printf "Transcripts_per_gene\t%.2f\n", t/g
}' "$GFF" | tee summary/transcripts_per_gene.tsv
```

### 5.6. Calculate CDS lengths per transcript

```bash
awk -F '\t' '
!/^#/ && $3=="CDS" && $9 ~ /(^|;)Parent=/ {
    attr=$9

    sub(/^.*Parent=/, "", attr)
    sub(/;.*/, "", attr)

    n=split(attr, parent, ",")
    len=$5-$4+1

    for (i=1; i<=n; i++) {
        gsub(/^ +| +$/, "", parent[i])

        if (parent[i] != "")
            cds_len[parent[i]] += len
    }
}
END {
    for (id in cds_len)
        print id "\t" cds_len[id]
}' "$GFF" \
| sort -k2,2n \
> summary/cds_lengths.tsv
```

### 5.7. Calculate mean and median CDS length

```bash
awk '
{
    value[NR]=$2
    sum+=$2
}
END {
    if (NR==0) {
        print "No CDS lengths found"
        exit 1
    }

    mean=sum/NR

    if (NR%2==1)
        median=value[(NR+1)/2]
    else
        median=(value[NR/2]+value[NR/2+1])/2

    printf "CDS_transcripts\t%d\n", NR
    printf "Mean_CDS_length_bp\t%.2f\n", mean
    printf "Median_CDS_length_bp\t%.2f\n", median
}' summary/cds_lengths.tsv \
| tee summary/cds_length_summary.tsv
```

### 5.8. Count short CDS models

```bash
awk '
$2 < 90  {lt90++}
$2 < 150 {lt150++}
$2 < 300 {lt300++}
END {
    print "CDS_lt_90_bp\t" lt90+0
    print "CDS_lt_150_bp\t" lt150+0
    print "CDS_lt_300_bp\t" lt300+0
}' summary/cds_lengths.tsv \
| tee summary/short_cds_counts.tsv
```

### 5.9. Calculate gene spans

```bash
awk -F '\t' '
!/^#/ && $3=="gene" {
    print $5-$4+1
}' "$GFF" \
| sort -n \
> summary/gene_spans.txt
```

### 5.10. Calculate mean and median gene span

```bash
awk '
{
    value[NR]=$1
    sum+=$1
}
END {
    if (NR==0) {
        print "No genes found"
        exit 1
    }

    mean=sum/NR

    if (NR%2==1)
        median=value[(NR+1)/2]
    else
        median=(value[NR/2]+value[NR/2+1])/2

    printf "Genes\t%d\n", NR
    printf "Mean_gene_span_bp\t%.2f\n", mean
    printf "Median_gene_span_bp\t%.2f\n", median
}' summary/gene_spans.txt \
| tee summary/gene_span_summary.tsv
```

## 6. Construct the longest isoform FASTA

One representative protein sequence is selected per gene by retaining the longest isoform.

```bash
python - <<'PY'
from pathlib import Path
import re

input_fasta = Path("NIBRbiolumScaffold.fa.proteins.fasta")
output_fasta = Path("summary/EviAnn.longest_isoform.faa")

best = {}
header = None
seq_parts = []


def save_record(header, seq_parts):
    if header is None:
        return

    protein_id = header.split()[0]
    sequence = "".join(seq_parts)
    gene_id = re.sub(r"-mRNA-\d+$", "", protein_id)

    if gene_id not in best or len(sequence) > len(best[gene_id][1]):
        best[gene_id] = (header, sequence)


with input_fasta.open() as handle:
    for line in handle:
        line = line.rstrip()

        if line.startswith(">"):
            save_record(header, seq_parts)
            header = line[1:]
            seq_parts = []
        else:
            seq_parts.append(line)

    save_record(header, seq_parts)


with output_fasta.open("w") as out:
    for header, sequence in best.values():
        out.write(f">{header}\n")

        for i in range(0, len(sequence), 60):
            out.write(sequence[i:i+60] + "\n")

print("Representative proteins:", len(best))
print("Output:", output_fasta)
PY
```

## 7. BUSCO assessment

### 7.1. Activate the BUSCO environment

```bash
conda activate busco_env
```

### 7.2. Run BUSCO using all predicted proteins

```bash
busco \
  -i NIBRbiolumScaffold.fa.proteins.fasta \
  -l fungi_odb12 \
  -m proteins \
  -o busco_eviann_all \
  -c 16
```

### 7.3. Run BUSCO using the longest isoform per gene

```bash
busco \
  -i summary/EviAnn.longest_isoform.faa \
  -l fungi_odb12 \
  -m proteins \
  -o busco_eviann_longest \
  -c 16
```
