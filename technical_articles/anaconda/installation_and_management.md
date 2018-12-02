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

