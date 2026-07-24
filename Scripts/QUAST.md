# QUAST
유전체 assembly 품질을 평가하는 도구  

assembled genome FASTA를 입력으로 받아서, assembly가 얼마나 길고 잘 이어져 있는지 통계로 보여줌.

1. quast용 가상 환경 만들기  
  ```text
  conda create -n quast_env -c conda-forge -c bioconda quast -y
  ```

2. Activate venv
```text
conda activate quast_env
```
3. Version check
```text
quast --version
```
If no error, next step

4. Run quast
```text
quast \
  ${PROJECT_DIR}/NIBRbiolumScaffold.fa \
  -o ${ANALYSIS_DIR}/quast_NIBR_biolum \
  -t 8 \
  --eukaryote
```
## Result
1. Open result
```text
cat ${ANALYSIS_DIR}/quast_NIBR_biolum/report.txt
```
### N50
긴 scaffold부터 더했을 때 전체 assembly 길이의 50%에 도달하게 만드는 scaffold의 길이
예시 결과 : N50 = 1,938,897 bp  

의미: 길이가 1,938,897 bp 이상인 긴 scaffold들을 합치면 전체 assembly 길이의 절반 이상을 차지한다는 뜻  
N50은 일반적으로 클수록 연속성이 높다.

### L50
전체 assembly 길이의 50%를 채우는 데 필요한 scaffold 개수  
예시 결과: L50 = 8

의미:가장 긴 scaffold 8개를 합하면 전체 assembly 길이의 50% 이상이 된다.  
L50은 일반적으로 작을수록 연속성이 높다.

### N90
긴 scaffold부터 더했을 때 전체 assembly 길이의 90%에 도달하게 만드는 scaffold의 길이  
예시 결과: N90 = 373,606 bp  

의미: 길이가 373,606 bp 이상인 scaffold들을 합하면 전체 assembly의 90% 이상을 차지한다.  
N90은 N50보다 항상 작거나 같다.

### L90
전체 assembly 길이의 90%를 채우는 데 필요한 scaffold 개수이다.  
예시 결과: L90 = 27  

의미: 가장 긴 scaffold 27개를 합하면 전체 assembly 길이의 90% 이상이 된다.

