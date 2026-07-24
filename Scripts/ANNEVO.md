# ANNEVO
## ANNEVO에 대해
ANNEVO는 딥러닝 기반 ab initio gene annotation method\
**ab initio gene annotation method**\
아무런 기존 실험 데이터(RNA 염기서열 등) 없이, 생물학적 법칙(수학적 모델)만을 이용해 DNA 서열 자체에서 유전자 위치를 컴퓨터로 예측하는 방법

### 입출력 데이터(Input & Output)
|구분|포맷|설명|
|:---|:---:|:---:|
|입력 (Input)|`.fasta` 또는 `.fa`|Assembled DNA 염기서열 데이터|
||`.ta`|saved_model 내부에 있는 ANNEVO_Fungi|
|출력 (Output)|`.gff3`|Annotation(exon/intron 좌표)된 유전체 데이터|

## 설치 및 구동 과정
### 설치
```Bash
# Installation
git clone https://github.com/xjtu-omics/ANNEVO.git
cd ANNEVO

# YAML 파일로 가상환경 자동 생성
conda env create -f ANNEVO.yml -n annevo_env
conda activate annevo_env

# CPU 전용 PyTorch로 덮어씌우기
conda install pytorch torchvision torchaudio cpuonly -c pytorch -y
```

### 구동
```Bash
#CPU 전용 실행 (환경 변수로 스레드 통제 및 GPU 숨김)
OMP_NUM_THREADS=$THREADS TF_NUM_INTRAOP_THREADS=$THREADS TF_NUM_INTEROP_THREADS=$THREADS CUDA_VISIBLE_DEVICES="" \
python annotation.py \
    -g PATH_게놈파일 \
    -m $PATH_모델파일 \
    -l Fungi \
    -o PATH_결과출력 \
    -t 25  # THREADS 지정
```


