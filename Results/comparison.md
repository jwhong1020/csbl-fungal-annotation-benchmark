# Comparison among models

| Model                  | Evidence                        |  Genes | Transcripts | Tx/gene | Exons/tx |    Mean CDS | Complete BUSCO | Fragmented |   Missing |
| ---------------------- | ------------------------------- | -----: | ----------: | ------: | -------: | ----------: | -------------: | ---------: | --------: |
| **BRAKER4**            | RNA-seq + protein               | 14,337 |      16,804 |    1.17 |     7.14 |    1,238 bp |      **91.4%** |       5.2% |  **3.5%** |
| **ANNEVO**             | 사전학습 모델                    |    13,108 |         13,108 |       1.00 |       6.5 | 1,278bp |          89.8% |       6.5% |      3.7% |
| **Helixer**            | 사전학습 모델                         | 15,304 |      15,304 |    1.00 |  약 6.45* | 1,239bp |          88.1% |       7.8% |      4.0% |
| **Tiberius ab initio** | genome only                     | 12,242 |      12,242 |    1.00 |     6.48 | 1,345.53 bp |          87.3% |       6.8% |      5.9% |
| **Tiberius evidence**  | RNA-seq + Fungi.fa              | 11,281 |      15,670 |    1.39 |     7.46 | 1,325.07 bp |         87.3%† |       6.8% |      5.9% |
| **EviAnn**             | RNA-seq + Omphalotaceae protein |  7,760 |      11,644 |    1.50 |     8.48 | 1,143.28 bp |      **58.6%** |  **16.5%** | **24.9%** |
