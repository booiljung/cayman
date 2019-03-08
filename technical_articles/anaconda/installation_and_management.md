# Installation and Management

## 업데이트

### 아나콘다 자체를 업데이트

```
conda update conda
```

### 아나콘다의 모든 패키지를 업데이트

```
conda update --all
```

### 머신러닝 연구용 설치 패키지 (ml)

아나콘다를 새로 설치할때마다 채널을 검색하기 귀찮아서 정리 합니다.

```sh
conda create -n ml python=3.6
source activate ml # Windows에서는 conda activate ml
conda install -c anaconda cudatoolkit
conda install -c anaconda numpy
conda install -c anaconda jupyter
```

#### datas

```sh
conda install -c anaconda pandas
```

#### visualization

```sh
conda install -c conda-forge matplotlib
conda install -c anaconda pil
```

#### pytorch

```sh
conda install -c pytorch pytorch
conda install -c pytorch torchvision
conda install -c derickl torchtext
```

#### opencv

```sh
conda install -c conda-forge opencv
```

#### tensorflow-gpu

```sh
conda install -c conda-forge tensorflow-gpu
```

#### keras-gpu

```sh
conda install -c anaconda keras-gpu
```

#### chainer

```sh
conda install -c anaconda chainer 
```

## env

### env 목록 보기

```sh
conda info --envs
```

### env 생성

```sh
conda create --name <env_name> python=<python_version>
```

파이썬 3.7이 사용된다면 `<python_version>`은  `python=3.7` 형식으로 지정합니다.

### env 선택 및 시작

리눅스

```sh
source activate <env_name>
```

### env 종료

리눅스

```sh
source deactivate
```

