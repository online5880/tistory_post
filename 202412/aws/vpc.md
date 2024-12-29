# AWS VPC란?

AWS VPC(Virtual Private Cloud)는 **AWS 클라우드에서 사용자가 정의한 가상 네트워크**임. 

**가상 네트워크가 핵심**

VPC는 사용자가 네트워크 구성을 제어할 수 있는 독립된 환경을 제공하며, **사용자 전용 네트워크를 구축**하는 데 사용됨. 

이 환경은 물리적 네트워크와 유사한 방식으로 작동하며, 사용자는 다음과 같은 요소를 설정 가능함:

- **서브넷(Subnet):** VPC 내부를 더 작은 네트워크로 나눌 수 있음.
- **라우팅 테이블(Routing Table):** 네트워크 트래픽이 어디로 이동할지 제어함.
- **인터넷 게이트웨이(Internet Gateway):** VPC를 인터넷에 연결하는 역할(외부와 통신).
- **NAT 게이트웨이(NAT Gateway):** 프라이빗 서브넷의 리소스가 인터넷에 액세스하도록 지원함(private ip -> public ip 바꿔줌).
- **보안 그룹(Security Groups) 및 네트워크 ACL(Access Control List):** 네트워크 트래픽을 제어하는 보안 계층.

<img src="https://github.com/online5880/tistory_post/blob/main/202412/aws/What is Amazon VPC.png?raw=true" width="100%" height="100%"/>

[출처(AWS:What is Amazon VPC?)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

---

## 주요 특징

### 1. **사용자 정의 네트워크**
VPC를 통해 **IP 주소 범위**, **라우팅 설정**, **인터넷 게이트웨이 또는 프라이빗 네트워크 구성**을 자유롭게 설정할 수 있음.

### 2. **보안 및 접근 제어**
- **보안 그룹**: 인스턴스 수준에서 트래픽을 제어함.
- **네트워크 ACL**: 서브넷 수준에서 트래픽을 제어함.
- **VPN 연결**: 온프레미스 데이터센터와 안전하게 연결 가능.

### 3. **확장성과 유연성**
AWS의 다른 서비스와 통합 가능하며, 네트워크 확장이 용이함. 예를 들어, EC2, RDS와 같은 리소스를 쉽게 배치 가능.

### 4. **가용성**
VPC는 여러 가용 영역(AZ, Availability Zone)을 지원하여 고가용성을 보장함.

---

## VPC 구성 요소

1. **서브넷(Subnet)**:
   - 퍼블릭 서브넷: 인터넷에 접근 가능한 리소스 배치.
   - 프라이빗 서브넷: 외부 접근이 차단된 리소스 배치.

2. **인터넷 게이트웨이(IGW)**:
   - 퍼블릭 서브넷의 리소스가 인터넷과 통신하도록 함.

3. **NAT 게이트웨이**:
   - 프라이빗 서브넷의 리소스가 인터넷으로 아웃바운드 트래픽을 보낼 수 있도록 함.

4. **라우팅 테이블**:
   - 네트워크 트래픽 경로를 정의.

5. **보안 그룹**:
   - 특정 리소스에 대한 인바운드/아웃바운드 트래픽 제어.

6. **VPC 피어링**:
   - 서로 다른 VPC 간의 통신을 허용.

- 이미지
    
    <img src="https://github.com/online5880/tistory_post/blob/main/202412/aws/vpc_dashboard.png?raw=true" width="30%" height="100%"/>

---

## 관련 링크
- [AWS VPC 공식 문서](https://aws.amazon.com/vpc/)
- [VPC 튜토리얼](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
