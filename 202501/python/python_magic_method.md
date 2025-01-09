
# Python 매직 메서드(Magic Method) 이해하기

파이썬에서 **매직 메서드(Magic Method)**는 클래스에 특별한 동작을 부여하기 위해 사용되는 특별한 형태의 메서드임. 

이러한 메서드는 더블 언더스코어(`__`)로 시작하고 끝나는 이름을 가지며, 객체의 특정 동작을 커스터마이즈하거나 오버라이딩할 수 있도록 설계됨. 예를 들어, `__init__`, `__str__`, `__add__` 등이 이에 해당함.

## 1. 매직 메서드의 기본 개념
매직 메서드는 클래스 내부에서 특별한 기능을 수행하며, Python의 빌트인 기능과 상호작용할 때 호출됨. 

직접 호출하기보다는 특정 상황에서 Python이 자동으로 호출함.

### 주요 매직 메서드 예시
- `__init__(self, ...)` : 객체 초기화
- `__str__(self)` : 객체의 문자열 표현 반환
- `__repr__(self)` : 객체의 공식 문자열 표현 반환
- `__add__(self, other)` : `+` 연산자 동작 정의
- `__len__(self)` : `len()` 함수 동작 정의
- `__getitem__(self, key)` : 객체에서 키로 값 접근 동작 정의

---

## 2. 주요 매직 메서드 살펴보기

### (1) `__init__` - 생성자
객체 생성 시 초기화 메서드임.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p = Person("Alice", 30)
print(p.name)  # 출력: Alice
```

---

### (2) `__str__`와 `__repr__` - 객체의 문자열 표현
- `__str__`: 사용자가 읽기 쉬운 문자열 반환
- `__repr__`: 개발자가 디버깅에 사용하는 문자열 반환

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"Person(name={self.name}, age={self.age})"

    def __repr__(self):
        return f"Person('{self.name}', {self.age})"

p = Person("Alice", 30)
print(str(p))   # 출력: Person(name=Alice, age=30)
print(repr(p))  # 출력: Person('Alice', 30)
```

---

### (3) 산술 연산 관련 메서드
산술 연산자(`+`, `-`, `*`, `/` 등)를 커스터마이즈할 수 있음.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)  # 출력: Vector(4, 6)
```

---

### (4) `__len__` - 길이 반환
`len()` 함수 호출 시 동작을 정의함.

```python
class CustomList:
    def __init__(self, items):
        self.items = items

    def __len__(self):
        return len(self.items)

cl = CustomList([1, 2, 3])
print(len(cl))  # 출력: 3
```

---

### (5) `__getitem__` - 인덱싱/슬라이싱
객체의 특정 값을 키 또는 인덱스로 접근할 수 있도록 설정함.

```python
class CustomList:
    def __init__(self, items):
        self.items = items

    def __getitem__(self, index):
        return self.items[index]

cl = CustomList([10, 20, 30])
print(cl[1])  # 출력: 20
```

---

## 3. 매직 메서드의 활용
매직 메서드는 객체 지향 프로그래밍에서 중요한 역할을 함. 코드의 가독성과 재사용성을 높이고, Pythonic한 클래스를 설계할 수 있음.

---

## 4. 자주 사용되는 매직 메서드 목록

| 메서드         | 역할                              |
|----------------|-----------------------------------|
| `__init__`     | 객체 초기화                      |
| `__str__`      | 객체의 사용자 친화적 문자열 반환 |
| `__repr__`     | 객체의 디버깅 문자열 반환        |
| `__add__`      | `+` 연산 동작 정의               |
| `__len__`      | `len()` 동작 정의                |
| `__getitem__`  | 객체에서 키/인덱스 접근 정의     |
| `__setitem__`  | 객체에서 키/인덱스 값 설정       |
| `__delitem__`  | 객체에서 키/인덱스 값 삭제       |
| `__call__`     | 객체를 함수처럼 호출 가능        |

---

## 5. 결론
Python의 매직 메서드는 객체 동작을 Pythonic하게 커스터마이즈할 수 있는 강력한 도구임. 

이를 활용하면 직관적이고 간결한 코드를 작성할 수 있음.
