
# macOS에서 파이썬 가상환경 설정 방법

macOS에서 파이썬 가상환경을 설정하는 방법에는 여러 가지가 있습니다. 

이 글에서는 `Anaconda`를 이용한 가상환경 설정 방법과 `프로젝트 내`에서 직접 가상환경을 만드는 방법 두 가지에 대해 설명하겠습니다.

## 1. Anaconda를 이용한 파이썬 가상환경 설정

### 1.1 아나콘다 설치 확인
아나콘다가 설치되어 있다는 가정하에 진행합니다. 만약 설치되지 않았다면, [Anaconda 공식 사이트](https://www.anaconda.com/products/individual)에서 설치할 수 있습니다.

### 1.2 파이썬 버전 확인
아나콘다의 파이썬 버전 목록을 확인하려면 다음 명령어를 사용합니다:

```bash
conda search python
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/conda_search.png?raw=true" width="50%" height="100%"/>


### 1.3 가상환경 생성
원하는 파이썬 버전을 지정하여 가상환경을 생성합니다. 예를 들어, `python 3.12.4` 버전을 사용하여 `Test`라는 이름의 가상환경을 만들고 싶다면 아래와 같이 명령어를 입력합니다:

```bash
conda create --name Test python==3.12.4
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/conda_create.png?raw=true" width="50%" height="100%"/>

### 1.4 가상환경 리스트 확인
현재 생성된 가상환경의 리스트는 다음 명령어로 확인할 수 있습니다:

```bash
conda info --envs
```


### 1.5 가상환경 활성화
가상환경을 활성화하려면 다음 명령어를 사용합니다:

```bash
conda activate [가상환경 이름]
```

예시:

```bash
conda activate Test
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/conda_activate.png?raw=true" width="50%" height="100%"/>


### 1.6 가상환경 비활성화
가상환경을 비활성화하려면 다음 명령어를 사용합니다:

```bash
conda deactivate
```

### 1.7 가상환경 삭제
필요 없는 가상환경을 삭제하려면 다음 명령어를 사용합니다:

```bash
conda remove --name [가상환경 이름] --all
```

예시:

```bash
conda remove --name Test --all
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/conda_remove.png?raw=true" width="100%" height="100%"/>

## 2. 프로젝트 내에 파이썬 가상환경 설정

### 2.1 가상환경 생성
프로젝트 내에 직접 가상환경을 만들 수 있습니다. 예를 들어 `test`라는 이름의 가상환경을 만들고 싶다면 다음 명령어를 사용합니다:

```bash
python3 -m venv test
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/venv_create.png?raw=true" width="50%" height="100%"/>

### 2.2 가상환경 활성화
가상환경을 활성화하려면 아래와 같이 명령어를 입력합니다:

```bash
source test/bin/activate
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/venv_active.png?raw=true" width="50%" height="100%"/>

### 2.3 가상환경 비활성화
가상환경을 비활성화하려면 다음 명령어를 사용합니다:

```bash
deactivate
```
<img src="https://github.com/online5880/tistory_post/blob/main/Images/venv/venv_decative.png?raw=true" width="50%" height="100%"/>

### 2.4 가상환경 삭제
프로젝트 내의 가상환경을 삭제하려면 다음 명령어를 사용합니다:

```bash
rm -rf [가상환경 이름]
```

예시:

```bash
rm -rf test
```
