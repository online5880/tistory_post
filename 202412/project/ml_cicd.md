# **AWS 기반 모델 학습 및 서빙 아키텍처**

---
<img src="https://github.com/online5880/tistory_post/blob/main/202412/project/ml_cicd.png?raw=true" width="100%" height="100%"/>

## **1. 개요**
아키텍처는 **AWS Cloud** 인프라를 활용하여 모델 학습과 배포를 자동화하는 구조입니다.  
GitHub Actions를 통해 학습 파이프라인을 실행하며, Docker와 MLflow를 사용하여 모델 서빙 환경을 구축하고 모델 관리를 수행합니다.  
최종적으로 Slack을 통해 알림을 전달하여 작업 상태를 공유합니다.

---

## **2. 주요 컴포넌트**

### **2.1 GitHub 및 GitHub Actions**
- **역할**: 모델 학습 및 배포 파이프라인을 자동화합니다.  
- **주요 동작**:
  - 학습 파이프라인 실행
  - AWS EC2 인스턴스에 접속하여 모델 학습 시작
  - 상태 알림을 Slack에 전송

---

### **2.2 Slack Notification**
- **역할**: GitHub Actions 파이프라인의 상태를 Slack에 알림으로 전달합니다.
- **주요 동작**:
  - 학습 시작 및 완료 알림
  - 모델 배포 성공/실패 알림
  
<img src="https://github.com/online5880/tistory_post/blob/main/202412/project/ml_slack.png?raw=true" width="100%" height="100%"/>

---

### **2.3 AWS EC2 (모델 서버)**
- **역할**: 모델 학습 및 서빙을 위한 핵심 인프라입니다.  
- **구성**:
  - **Docker**: MLflow와 Python 환경을 컨테이너로 배포하여 모델 학습 및 서빙을 진행합니다.
  - **MLflow**: 모델 학습 결과를 추적하고, 모델을 버전 관리합니다.  
    - **Model Server (포트 :5000)**: 학습된 모델을 서버에 저장하고 모델 서빙 준비를 합니다.  
    - **Model Serving (포트 :5001)**: 학습된 모델을 서빙하여 외부 요청을 처리합니다.  
  - **Python (Boto3)**: AWS SDK를 사용하여 S3 버킷과 통신합니다.

---

### **2.4 AWS S3 (모델 저장소)**
- **역할**: 학습된 모델 파일을 저장하는 스토리지입니다.  
- **주요 동작**:
  - MLflow를 통해 저장된 모델 아티팩트를 S3 버킷에 업로드
  - 서빙 시 모델을 S3에서 불러와 EC2 인스턴스에서 서빙

---

## **3. 아키텍처 동작 흐름**

1. **모델 학습 (Training)**:
   - GitHub Actions가 GitHub Runner를 통해 학습 스크립트를 실행합니다.  
   - EC2 인스턴스에서 Docker를 통해 MLflow 환경을 설정하고 학습을 수행합니다.  
   - 학습된 모델은 MLflow를 통해 AWS S3에 저장됩니다.

2. **모델 서빙 (Model Serving)**:
   - MLflow가 EC2 인스턴스 내 **Model Server**와 **Model Serving** 서비스를 제공하며,  
     외부 포트(`:5000`, `:5001`)를 통해 모델 요청을 처리합니다.  
   - 서빙 시 S3 버킷에서 최신 모델을 불러옵니다.

3. **Slack 알림**:
   - GitHub Actions는 학습 및 서빙 상태를 Slack에 알림으로 전달합니다.

---

## **4. 주요 기술 스택**
| **기능**            | **기술**         |
|--------------------|-----------------|
| **CI/CD**          | GitHub Actions  |
| **컨테이너화**       | Docker          |
| **모델 관리 및 서빙** | MLflow          |
| **모델 저장소**       | AWS S3          |
| **인프라**           | AWS EC2         |
| **스크립트 실행 및 S3 통신** | Python (Boto3) |
| **알림**            | Slack           |

---

## **5. 요약**
아키텍처는 모델 학습과 서빙 파이프라인을 효율적으로 자동화합니다.  
GitHub Actions를 통해 학습 및 서빙 작업을 관리하고, Docker와 MLflow를 통해 모델을 버전 관리하며 AWS S3에 저장합니다.  
최종적으로 Slack을 통해 상태를 모니터링하여 협업과 운영 효율성을 극대화합니다.
