# 데이터베이스 네이밍 규칙

데이터베이스 설계 시 네이밍 규칙을 일관성 있게 적용하면 가독성을 높이고, 협업 및 유지보수를 용이하게 할 수 있음. 

아래는 효율적인 데이터베이스 네이밍 규칙을 정리한 내용임

## 1. 테이블명과 컬럼명을 **소문자**로 작성
- 테이블명과 컬럼명을 대소문자를 혼용하지 않고, **소문자**로 작성  
- 운영체제 및 데이터베이스 환경에 따라 대소문자 처리 방식이 다를 수 있음
- 예시:  
  SELECT user_id, user_name FROM users;

---

## 2. **snake_case** 사용
- 단어 간 띄워쓰기를 할 때 **snake_case**를 사용 
  - [참고 : 스네이크 케이스(Snake case) - MDN Web Docs 용어 사전](https://developer.mozilla.org/ko/docs/Glossary/Snake_case)  
- 가독성을 높이고 일관성을 유지함
- 일부 데이터베이스나 툴에서는 공백을 제대로 처리하지 못하므로 이를 방지  
- 예시:  
  - user_id, order_date

---

## 3. 축약어 사용 지양
- 축약어를 사용하지 않아, 누구나 쉽게 이해할 수 있도록 **직관적**으로 작성  
- 협업 환경에서 코드 가독성을 높이고 유지보수를 용이하게 함  
- 예시:  
  - Bad: usr_id, prd_nm  
  - Good: user_id, product_name

---

## 4. SQL문 작성 시 예약어는 대문자로 작성
- SQL 문법에서 예약어는 **대문자**로 작성하여 가독성을 높임  
- 예시:
    ```sql  
    SELECT user_id, user_name, email 
    FROM users 
    WHERE status = 'active';
    ```

---

## 5. 테이블 명에 복수형 사용 (선택 사항)
- 테이블에는 보통 다수의 데이터가 포함되므로 **복수형**을 사용  
- 복수형 또는 단수형 중 하나를 일관성 있게 사용  
- 예시:  
  - 복수형: users, orders  
  - 단수형: user, order

---

## 네이밍 규칙 적용 예제
### 테이블: users (복수형, snake_case 사용)
컬럼명       | 설명             
--------------|------------------
id            | 사용자 ID (Primary Key)         
user_name     | 사용자 이름      
user_email    | 사용자 이메일 (Unique Key)    
created_at    | 생성 일시      

### SQL 예제

```sql
SELECT user_id, user_name, email 
FROM users 
WHERE status = 'active';
```