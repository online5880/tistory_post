
# [CI/CD] Docker, EC2, ECR, CodeDeploy, Github Actions를 활용한 배포 자동화

이 글에서는 Docker, EC2, ECR, CodeDeploy, Github Actions를 조합하여 애플리케이션 배포를 자동화하는 방법을 단계별로 설명함. 

이를 통해 지속적 통합(CI)과 지속적 배포(CD)를 구현할 수 있음.

---

## CI/CD 파이프라인 프로세스 설명

<img src="https://github.com/online5880/tistory_post/blob/main/202412/devops/CodeDeploy_CICD.png?raw=true" width="100%" height="100%"/>

CI/CD 파이프라인의 주요 단계를 아래와 같이 설명합니다:

1. **Git Push**  
   개발자가 GitHub 저장소에 변경 사항을 Push하면 CI/CD 프로세스가 시작

2. **Docker 이미지 생성**  
   GitHub Actions가 트리거되며, 애플리케이션 코드를 기반으로 Docker 이미지를 생성

3. **Docker 이미지를 ECR로 Push**  
   생성된 Docker 이미지는 AWS ECR(Elastic Container Registry)에 업로드. 이는 컨테이너 이미지를 안전하게 저장하고 관리하기 위함.

4. **CodeDeploy 관련 파일 S3 업로드**  
   애플리케이션 배포를 위한 `appspec.yml` 파일과 스크립트를 압축하여 AWS S3 버킷에 업로드.

5. **CodeDeploy 배포 명령 실행**  
   AWS CodeDeploy를 사용해 EC2 인스턴스에 배포 프로세스를 시작.

6. **EC2 인스턴스에서 S3 파일 다운로드**  
   EC2 인스턴스는 S3에서 필요한 파일을 다운로드하여 배포 준비.

7. **CodeDeploy를 통해 배포 진행**  
   EC2 인스턴스는 다운로드한 파일을 활용하여 배포 작업 실행.

8. **Docker 이미지를 EC2에 Pull 및 배포**  
   EC2 인스턴스는 ECR에서 Docker 이미지를 Pull하고, 이를 실행하여 컨테이너 기반 애플리케이션 배포.

---

## 1. EC2에 Docker 설치

Docker를 설치하여 EC2 환경에서 컨테이너 기반 애플리케이션을 실행할 수 있도록 준비함. 아래 명령어를 사용하여 설치 진행:

```bash
sudo apt-get update && \
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
    sudo apt-key fingerprint 0EBFCD88 && \
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    sudo apt-get update && \
    sudo apt-get install -y docker-ce && \
    sudo usermod -aG docker ubuntu && \
    sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    sudo chmod +x /usr/local/bin/docker-compose && \
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

---

## 2. IAM 권한 설정

ECR과 연동하려면 IAM 역할에 필요한 권한을 부여해야 함. 다음 권한을 추가:

- **`AmazonEC2ContainerRegistryFullAccess`**

---

## 3. ECR 레포지토리 생성

AWS Management Console에서 **ECR** 서비스로 이동하여 새로운 레포지토리를 생성.

---

## 4. Dockerfile 생성

애플리케이션 이미지를 빌드하기 위한 Dockerfile을 생성:

```dockerfile
# Example Dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## 5. Amazon ECR Docker Credential Helper 설치

Amazon ECR과 Docker를 통합하려면 Docker Credential Helper를 설치해야 함. 다음 명령어 실행:

```bash
sudo apt install amazon-ecr-credential-helper
```

설치 후 `~/.docker/config.json` 파일에 다음 내용을 추가하여 설정:

```json
{
    "credsStore": "ecr-login"
}
```

---

## Github Actions 워크플로우 작성

아래는 Github Actions 워크플로우의 주요 과정과 설정 방법을 나타냄:

### 1. ECR에 로그인

`aws-actions/amazon-ecr-login` 액션을 사용하여 AWS ECR에 로그인:

```yaml
- name: ECR에 로그인하기
  id: login-ecr
  uses: aws-actions/amazon-ecr-login@v2
```

---

### 2. Docker 이미지 빌드 및 태그 생성

Docker 이미지를 빌드하고 태그를 생성하여 ECR에 푸시할 준비를 완료:

```yaml
- name: Docker 이미지 생성
  run: docker build -t [이미지이름] .

- name: Docker 이미지에 Tag 붙이기
  run: docker tag [이미지이름] ${{ steps.login-ecr.outputs.registry }}/[이미지이름]:latest
```

---

### 3. Docker 이미지를 ECR에 푸시

생성된 Docker 이미지를 AWS ECR로 푸시:

```yaml
- name: ECR에 Docker [이미지이름] Push
  run: docker push ${{ steps.login-ecr.outputs.registry }}/[이미지이름]:latest
```

---

### 4. 프로젝트 압축 및 S3 업로드

배포에 필요한 프로젝트 파일(appspec.yml, 스크립트 등)을 tar로 압축한 후, AWS S3 버킷에 업로드:

```yaml
- name: 압축하기
  run: tar -czvf $GITHUB_SHA.tar.gz appspec.yml scripts

- name: S3 에 프로젝트 폴더 업로드 하기
  run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.tar.gz s3://[S3이름]/$GITHUB_SHA.tar.gz
```

---

### 5. CodeDeploy로 EC2에 배포

AWS CodeDeploy를 활용하여 EC2로 프로젝트를 배포:

```yaml
- name: CodeDeploy를 활용해 EC2에 프로젝트 코드 배포
  run: aws deploy create-deployment \
    --application-name [애플리케이션이름] \
    --deployment-group-name [그룹이름] \
    --deployment-config-name CodeDeployDefault.AllAtOnce \
    --s3-location bucket=[S3버킷이름],bundleType=tgz,key=$GITHUB_SHA.tar.gz
```

---

## 전체 워크플로우 코드

아래는 Github Actions 워크플로우 전체 YAML 파일 예시:

```yaml
name: CI/CD for Docker, EC2, ECR, and CodeDeploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout 코드
        uses: actions/checkout@v3

      - name: AWS 인증
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2

      - name: ECR에 로그인하기
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Docker 이미지 생성
        run: docker build -t [이미지이름] .

      - name: Docker 이미지에 Tag 붙이기
        run: docker tag [이미지이름] ${{ steps.login-ecr.outputs.registry }}/[이미지이름]:latest

      - name: ECR에 Docker [이미지이름] Push
        run: docker push ${{ steps.login-ecr.outputs.registry }}/[이미지이름]:latest

      - name: 프로젝트 파일 압축
        run: tar -czvf $GITHUB_SHA.tar.gz appspec.yml scripts

      - name: S3에 프로젝트 파일 업로드
        run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.tar.gz s3://[S3버킷이름]/$GITHUB_SHA.tar.gz

      - name: CodeDeploy로 배포
        run: aws deploy create-deployment \
          --application-name [애플리케이션이름] \
          --deployment-group-name [그룹이름] \
          --deployment-config-name CodeDeployDefault.AllAtOnce \
          --s3-location bucket=[S3버킷이름],bundleType=tgz,key=$GITHUB_SHA.tar.gz
```

---

## 마무리

위 과정을 통해 Docker, EC2, ECR, CodeDeploy, Github Actions를 조합한 CI/CD 파이프라인을 완성할 수 있음. 이를 통해 애플리케이션 배포 과정을 자동화하여 신속하고 안정적인 배포를 구현 가능.

