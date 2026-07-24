# How to run BUSCO
### 1. Activate the BUSCO environment

```bash
conda activate busco_env
```

### 2. Run BUSCO using all predicted proteins
Replace -i into your input file.  
Replace -o into your output file.  
We used fungi_odb12 in BUSCO assessment.


```bash
busco \
  -i NIBRbiolumScaffold.fa.proteins.fasta \
  -l fungi_odb12 \
  -m proteins \
  -o busco_eviann_all \
  -c 16
```

### 3. Run BUSCO using the longest isoform per gene

```bash
busco \
  -i path/{Your.longest_isoform}.faa \
  -l fungi_odb12 \
  -m proteins \
  -o {busco_longest} \
  -c 16
```
