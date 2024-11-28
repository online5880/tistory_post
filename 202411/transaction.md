## 트랜잭션
- 데이터베이스에서 하나의 작업 단위로 처리하는 SQL 쿼리들의 집합
- 모든 쿼리가 성공하거나 실패하는 "All or Nothing" 원칙

## 트랜잭션 주요 단계
- Transaction `Begin`
  - 트랜잭션의 시작을 알림.
  - 여러 SQL 작업이 묶이게 된다.
- Transaction `COMMIT`
  - 트랜잭션 내에서 발생한 모든 변경 사항을 데이터베이스에 영구적으로 저장
- Transaction `ROLLBACK`
  - 트랜잭션 도중 오류가 발생하거나 취소하고 싶을 때, 모든 변경 사항을 초기 상태로 복구
- 예상치 못한 종료
  - 시스템이 갑자기 종료되거나 충돌 시, 트랜잭션은 자동으로 `ROLLBACK`  처리

## 커밋 중 시스템이 다운된다면?
- 큰 트랜잭션을 할 경우 커밋 중에 시스템이 다운될 가능성이 더 크다.
- 커밋이 빠르다면 시스템이 다운될 가능성이 낮아진다.
- 해결 방법:
  - 큰 트랜잭션은 여러 작은 트랜잭션으로 나누기

## 읽기 전용 트랜잭션(Read Only Transaction)
- 데이터를 변경하지 않고 조회만 하는 트랜잭션

## 트랜잭션의 기본 동작
- 단일 쿼리도 기본적으로 트랜잭션 안에서 실행된다.
  - 예: `UPDATE users SET balance = balance - 100 WHERE id = 1;` 이것도 트랜잭션이다.
- 여러 쿼리를 묶어서 처리하려면 명시적으로 트랜잭션을 선언해야한다.
```SQL
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

--- 
## ROLLBACK 은 명시적으로 선언해야하나?
- 맞다고 한다.
- 실패하거나 오류가 발생할 가능성이 있는 부분에서 선언한다.
- 프로그래밍에서 예외 처리라고 생각하면 편할듯하다.