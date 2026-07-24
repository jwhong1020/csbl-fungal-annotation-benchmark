# Assembly quality assessment
All models used the same genome assembly. 
Therefore, the quality and continuity of the assembly were assessed before comparing the annotation results.  

## Assembly continuity 
Assembly continuity was evaluated using QUAST. The NIBRbiolumScaffold assembly had a total length of 38.40 Mb and consisted of 63 sequences. The largest scaffold was 3.61 Mb, with an N50 of 1.94 Mb and an L50 of 8. The assembly had a GC content of 46.78%.  

| Metric              |                 Result |
| ------------------- | ---------------------: |
| Assembly size       |               38.40 Mb |
| Number of sequences |                     63 |
| Largest scaffold    |                3.61 Mb |
| N50                 |                1.94 Mb |
| L50                 |                      8 |
| N90                 |               373.6 kb |
| L90                 |                     27 |
| GC content          |                 46.78% |
| N content           | 1,652.74 Ns per 100 kb |


### Comparison with Omphalotus guepiniiformis genome in NCBI
accession code : PRJNA886877  
Omphalotus guepiniiformis has been treated as a synonym of O. japonicus in Korean taxonomic literature, although database nomenclature may vary.  


| Metric             |        NIBR_biolum | NCBI reference |
| ------------------ | -----------------: | -------------: |
| Genome size        |            38,395,153 bp  |  42,513,208 bp
| Number of sequences |           63 scaffolds   |  80 contigs
| N50                |            1,938,897 bp   |   1,411,604 bp
| GC content         |            46.78%         |  46.52%
| Complete BUSCO     |            98.8%          |  95.7%
| Predicted coding genes |         14,337        |   15,554
| Mean gene/genomic span  |       1,668 bp       |  1,972 bp
| Mean exons              |       7.14/transcript |   5.57/gene

BRAKER4 predicted 1,217 fewer genes than the published annotation.
This difference may reflect differences in assembly content, repeat
masking, gene-prediction methods, evidence data, and filtering
criteria.  
Gene-coordinate and orthology analyses are required to
determine whether the difference represents missing genes, merged
gene models, or exclusion of short and weakly supported predictions.
