
# 네트워크 계층 모델 정리: OSI 7계층

## 1. 물리 계층(Physical Layer)

- **역할**: 장치를 연결하기 위한 매체의 물리적인 사항을 정의
    - 전압, 주기, 시간, 전선의 규격, 거리 등
- **주요 단위**: Bits (0과 1로 구성)
- **대표 구성 요소**
    - **허브 리피터**
        - 허브
            - 다수의 기기를 연결하는 장치
            - 특징:
                - 에러, 충돌, 디바이스별 제어 기능 없음
                - 받은 내용을 그대로 전달 → 무조건 Broadcast
            - 해결하지 못한 문제:
                - 충돌
                - Broadcast로 인한 비효율성

---

## 2. 데이터 링크 계층(Data Link Layer)

- **역할**: 디바이스 간 통신 및 전송 안정화를 위한 프로토콜 정의
- **주요 단위**: Frame
- **주요 구성 요소**
    - **MAC Address**
        - 네트워크 인터페이스의 고유 주소
        - 48비트(6바이트), 예: `00:1A:2B:3C:4D:5E`
        - OUI(제조사 식별자)와 NIC(인터페이스 고유 번호)로 구성
    - **스위치**
        - MAC Address를 기반으로 데이터를 전달
- **특징**
    - CSMA/CD 방식으로 통신 제어
    - Unicast를 통해 대상을 구별하여 통신 지원
- **해결하지 못한 문제**
    - 로컬 네트워크 외부와의 통신 불가

---

## 3. 네트워크 계층(Network Layer)

- **역할**: 경로 설정 및 데이터 전달 기능 제공
- **주요 단위**: 패킷
- **주요 구성 요소**
    - **IP**
        - 통신 주체 식별을 위한 주소
        - IPv4(32비트)와 IPv6(128비트)
    - **라우터**
        - 네트워크 간 패킷 전달
        - 경로 최적화를 위해 라우팅 테이블 사용
    - **ARP**
        - IP를 MAC Address로 변환
- **특징**
    - Local Network 간 통신 지원
    - CIDR 및 Subnet Mask 활용
- **해결하지 못한 문제**
    - 동시 통신 불가, 데이터 순서 보장 및 유실 대응 불가

---

## 4. 전송 계층(Transport Layer)

- **역할**: 데이터 전달의 신뢰성 확보
- **주요 단위**: 세그먼트
- **주요 구성 요소**
    - **TCP**
        - 데이터 무결성 및 순서 보장
        - 연결 지향
        - 사용 사례: HTTP, 이메일, 파일 전송 등
    - **UDP**
        - 빠르고 간단한 데이터 전달
        - 연결 비지향
        - 사용 사례: 스트리밍, VoIP, 온라인 게임
- **TCP Handshake**
    - 3단계 과정(SYN, SYN-ACK, ACK)을 통해 연결 수립

---

## 5. 세션 계층(Session Layer)

- **역할**: 통신 주체 간 연결 유지 방법 정의
- **사례**: HTTP Cookie
- 일부 프로토콜(FTP 등)은 세션 계층 구현 안 함

---

## 6. 표현 계층(Presentation Layer)

- **역할**: 데이터 해석 및 변환
    - 파싱, 압축 해제, 복호화 등

---

## 7. 응용 계층(Application Layer)

- **역할**: 데이터 처리 방법 정의