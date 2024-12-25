
# GitHub Actions에서 AWS Resource 접근을 위한 AWS Credentials 설정

GitHub Actions를 사용하여 AWS 리소스에 접근하려면 AWS 자격 증명(AWS credentials)을 설정해야 합니다.

이를 위해 `aws-actions/configure-aws-credentials` 액션을 사용할 수 있습니다. 아래는 설정 방법에 대한 간단한 예시 코드합니다.

## 설정 코드

아래 코드는 GitHub Actions 워크플로에서 AWS 자격 증명을 설정하는 방법을 보여줍니다.
이 설정은 AWS 리소스와 상호작용하는 작업(예: S3 업로드, Lambda 배포 등)을 수행할 때 필요합니다.

```yaml
- name: AWS Resource에 접근할 수 있게 AWS credentials 설정
  uses: aws-actions/configure-aws-credentials@v4
  with:
    aws-region: ap-northeast-2
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## 주요 구성 요소 설명

1. **`uses`**: `aws-actions/configure-aws-credentials@v4`  
   AWS 리소스와 통신하기 위한 공식 GitHub 액션을 사용합니다.

2. **`with`**:
   - `aws-region`: AWS 리소스가 위치한 리전. 예: `ap-northeast-2`는 서울 리전을 나타냄.
   - `aws-access-key-id` 및 `aws-secret-access-key`: AWS 인증에 필요한 자격 증명. GitHub Secrets에 저장된 키를 참조하여 보안을 유지합니다.

## 보안 주의사항

- **Secrets 사용**: `AWS_ACCESS_KEY_ID`와 `AWS_SECRET_ACCESS_KEY`는 GitHub Repository의 Secrets에 저장하고 참조해야 합니다.
  GitHub Secrets은 민감한 정보를 안전하게 저장할 수 있도록 제공되는 기능입니다.
  
  예를 들어, Secrets 설정은 GitHub Repository > Settings > Secrets and variables > Actions 메뉴에서 추가 가능

## 참고 자료

- [AWS GitHub Actions 공식 문서](https://github.com/aws-actions/configure-aws-credentials)

