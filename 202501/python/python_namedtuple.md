
# 파이썬 `namedtuple`에 대한 이해와 활용

파이썬의 `namedtuple`은 **간단하고 가벼운 데이터 객체를 생성**할 수 있는 표준 라이브러리 모듈인 `collections`에서 제공됨. 

기존의 튜플과 유사하지만, **필드 이름을 지정**하여 가독성을 높이고 키워드로 접근할 수 있는 점이 특징임. 

---

## 1. `namedtuple`의 기본 사용법

`namedtuple`은 튜플과 같은 불변 객체이지만, 필드 이름으로 값에 접근 가능함.

### `namedtuple` 정의

```python
from collections import namedtuple

# Person이라는 namedtuple 정의
Person = namedtuple('Person', ['name', 'age', 'city'])

# 인스턴스 생성
p = Person(name='Alice', age=30, city='Seoul')

# 필드 접근
print(p.name)  # 출력: Alice
print(p.age)   # 출력: 30
print(p.city)  # 출력: Seoul
```

---

## 2. 주요 특징과 장점

### (1) 가독성 향상
- 일반 튜플: 인덱스로 값 접근 → 가독성 떨어짐.
- `namedtuple`: 이름으로 값 접근 → 의미 명확.

```python
# 일반 튜플 예시
person_tuple = ('Alice', 30, 'Seoul')
print(person_tuple[0])  # Alice

# namedtuple
print(p.name)  # Alice
```

### (2) 불변성 유지
`namedtuple`은 기본적으로 불변 객체로, 값 변경이 불가능함. 이는 데이터 무결성을 유지하는 데 도움을 줌.

---

## 3. `namedtuple`의 다양한 기능

### (1) 기본값 지정
`namedtuple`의 필드에 기본값을 설정하려면 `_fields_defaults`를 활용함.

```python
from collections import namedtuple

# 기본값이 있는 namedtuple 생성
Person = namedtuple('Person', ['name', 'age', 'city'])
Person.__new__.__defaults__ = ('Unknown', 0, 'Unknown')

p = Person(name='Alice')
print(p)  # 출력: Person(name='Alice', age=0, city='Unknown')
```

---

### (2) 딕셔너리로 변환
`namedtuple` 객체를 딕셔너리로 변환할 수 있음.

```python
p = Person(name='Alice', age=30, city='Seoul')
print(p._asdict())  # 출력: {'name': 'Alice', 'age': 30, 'city': 'Seoul'}
```

---

### (3) 값 업데이트
새로운 값을 가진 `namedtuple` 객체를 생성하려면 `_replace()` 메서드를 사용함.

```python
updated_p = p._replace(city='Busan')
print(updated_p)  # 출력: Person(name='Alice', age=30, city='Busan')
```

---

### (4) 필드 이름 확인
`_fields` 속성을 사용하여 필드 이름을 확인할 수 있음.

```python
print(Person._fields)  # 출력: ('name', 'age', 'city')
```

---

### (5) 추가 메서드와 속성
- `_make(iterable)`: iterable로부터 namedtuple 생성.
- `_asdict()`: 딕셔너리 변환.
- `_replace()`: 값을 대체한 새로운 객체 반환.

```python
data = ['Bob', 25, 'Incheon']
new_person = Person._make(data)
print(new_person)  # 출력: Person(name='Bob', age=25, city='Incheon')
```

---

## 4. `namedtuple`의 활용 사례

### (1) 간단한 데이터 모델링
예를 들어, 좌표를 나타낼 때 `namedtuple`을 사용하면 코드 가독성을 높일 수 있음.

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p1 = Point(10, 20)
p2 = Point(30, 40)

print(p1.x + p2.x)  # 출력: 40
```

---

### (2) 함수 반환값에 활용
여러 값을 반환하는 함수에서 튜플 대신 `namedtuple`을 사용하면 의미가 명확해짐.

```python
from collections import namedtuple

def get_user_info():
    UserInfo = namedtuple('UserInfo', ['name', 'age', 'email'])
    return UserInfo(name='Alice', age=30, email='alice@example.com')

user = get_user_info()
print(user.name)  # 출력: Alice
print(user.email) # 출력: alice@example.com
```

---

### (3) 로그 또는 데이터 처리
`namedtuple`을 사용하면 로그 데이터나 데이터를 구조화하여 처리하기 쉬움.

```python
from collections import namedtuple

Log = namedtuple('Log', ['timestamp', 'level', 'message'])

log_entry = Log(timestamp='2025-01-05 10:00:00', level='INFO', message='System started.')
print(log_entry.message)  # 출력: System started.
```

---

## 5. 결론

`namedtuple`은 간단한 데이터 객체를 생성하는 데 유용하며, 코드 가독성과 효율성을 높이는 데 기여함. 특히 함수의 반환값이나 가벼운 데이터 구조에 적합함.

---

## 관련 태그
#Python #namedtuple #파이썬기초 #데이터구조 #객체지향 #PythonTips #데이터처리 #프로그래밍 #파이썬활용 #코딩
