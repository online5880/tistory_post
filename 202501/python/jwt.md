# Python으로 JWT 인증 서비스 구현하기

JWT(Json Web Token)는 웹 애플리케이션에서 인증(Authentication)과 권한 부여(Authorization)를 위해 널리 사용되는 토큰 기반 인증 방식임. 

Python을 사용하여 JWT를 생성하고 검증하는 간단한 `AuthenticationService` 클래스를 구현하는 방법을 소개함. 

이 코드는 Django 프로젝트 환경을 가정하며 `PyJWT` 라이브러리를 사용함.

---

## JWT란?

JWT는 JSON 형식의 데이터를 Base64로 인코딩한 문자열로, 안전한 정보 교환을 위해 주로 사용됨. JWT는 크게 세 가지 부분으로 구성됨:
1. **Header**: 토큰 유형과 알고리즘 정보
2. **Payload**: 사용자 정보 및 클레임(Claims)
3. **Signature**: 토큰 무결성을 보장하는 서명

---

## 구현 코드

아래 코드는 JWT를 생성하고 검증하는 Python 클래스를 구현한 예제임.

```python
import time
import jwt
from typing import TypedDict, ClassVar
from django.conf import settings


class JWTPayload(TypedDict):
    user_id: int
    exp: int


class NotAuthorizedException(Exception):
    """인증 실패 예외"""
    pass


class AuthenticationService:
    JWT_SECRET_KEY: ClassVar[str] = settings.SECRET_KEY
    JWT_ALGORITHM: ClassVar[str] = "HS256"

    @staticmethod
    def _unix_timestamp(second_in_future: int) -> int:
        """현재 시간으로부터 지정된 초만큼 더한 Unix 타임스탬프를 반환"""
        return int(time.time()) + second_in_future

    def encode_token(self, user_id: int) -> str:
        """
        JWT 토큰을 생성함
        :param user_id: 토큰에 포함할 사용자 ID
        :return: 생성된 JWT 토큰
        """
        return jwt.encode(
            {
                "user_id": user_id,
                "exp": self._unix_timestamp(second_in_future=24 * 60 * 60),  # 24시간 후 만료
            },
            self.JWT_SECRET_KEY,
            algorithm=self.JWT_ALGORITHM,
        )

    def verify_token(self, jwt_token: str) -> int:
        """
        JWT 토큰을 검증함
        :param jwt_token: 클라이언트가 제공한 JWT 토큰
        :return: 검증된 사용자 ID
        """
        try:
            payload: JWTPayload = jwt.decode(
                jwt_token, self.JWT_SECRET_KEY, algorithms=[self.JWT_ALGORITHM]
            )
            user_id: int = payload["user_id"]
            exp: int = payload["exp"]
        except Exception:
            raise NotAuthorizedException

        if exp < self._unix_timestamp(second_in_future=0):
            raise NotAuthorizedException
        return user_id
```

---

## 주요 함수 설명

1. **_unix_timestamp**
   - 현재 시간을 기준으로 몇 초 뒤의 Unix 타임스탬프를 반환
   - exp(만료 시간)를 계산하는 데 사용됨

2. **encode_token**
   - 사용자 ID와 만료 시간을 포함하는 JWT를 생성함
   - 기본 만료 시간은 24시간으로 설정됨

3. **verify_token**
   - 전달된 JWT 토큰의 유효성을 검증함
   - exp 값이 현재 시간보다 과거일 경우 인증 실패 예외를 발생시킴

---

## 예외 처리

- verify_token 메서드는 인증 실패 시 NotAuthorizedException을 발생시킴. 이를 통해 비정상적인 요청을 처리 가능함

```
class NotAuthorizedException(Exception):
    """인증 실패 예외"""
    pass
```

---

## 코드 활용 방법

### 1. JWT 생성

```python
auth_service = AuthenticationService()
jwt_token = auth_service.encode_token(user_id=123)
print(jwt_token)  # 생성된 JWT 출력
```

### 2. JWT 검증

```python
try:
    user_id = auth_service.verify_token(jwt_token=jwt_token)
    print(f"인증된 사용자 ID: {user_id}")
except NotAuthorizedException:
    print("인증 실패: 유효하지 않은 토큰")
```

---

## PyJWT 설치 방법

JWT 처리를 위해 PyJWT 라이브러리를 설치해야 함

```bash
pip install PyJWT
```
---

## 결론

JWT는 사용자 인증과 세션 관리를 효과적으로 처리할 수 있는 강력한 도구임. 

이번 글에서는 Django 설정과 PyJWT 라이브러리를 활용하여 JWT 생성 및 검증 클래스를 구현해보았음. 

이를 기반으로 더욱 강력한 인증 시스템을 구축할 수 있음
