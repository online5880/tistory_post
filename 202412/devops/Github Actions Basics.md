
# Github Actions 기본 문법 정리

Github Actions를 활용하여 간단한 Workflow를 실행하는 방법을 정리합니다.

---

## Workflow 설정

### Workflow의 이름

```yaml
name: Github Actions 실행시켜보기
```

### Event: 실행 시점 설정

- `main` 브랜치에 `push`될 때 Workflow 실행.

```yaml
on:
  push:
    branches:
      - main
```

---

## Job 구성

Workflow는 1개 이상의 Job으로 구성됩니다. 여러 Job은 기본적으로 병렬적으로 수행됩니다.

### Job 정의

- Job ID: `My-Deploy-Job`
- 실행 환경: `ubuntu-latest`

```yaml
jobs:
  My-Deploy-Job:
    runs-on: ubuntu-latest
    steps:
```

---

## Step 구성

### Step 1: 간단한 명령어 실행

```yaml
      - name: Hello World 찍기
        run: echo "Hello World"
```

### Step 2: 여러 명령어 실행

- `|`를 사용해 여러 문장을 작성할 수 있습니다.

```yaml
      - name: 여러 명령어 문장 작성하기
        run: |
          echo "굿"
          echo "모닝"
```

### Step 3: Github Actions 내장 변수 사용

- `$`를 사용하여 리눅스에서 변수를 활용할 수 있습니다.

```yaml
      - name: Github Actions 자체에 저장되어 있는 변수 사용해보기
        run: |
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
```

### Step 4: Github Actions Secret 활용

- `${{secrets.<SECRET_NAME>}}`를 통해 민감한 정보를 노출하지 않도록 설정할 수 있습니다.

```yaml
      - name: 아무한테나 노출이 되면 안되는 값
        run: |
          echo ${{secrets.MY_NAME}}
          echo ${{secrets.MY_HOBBY}}
```
