# Python Typing이란?

Python의 Typing은 변수, 함수, 클래스 등에 데이터 타입을 명시적으로 지정하는 기능임. 

Python은 기본적으로 동적 타이핑(dynamic typing) 언어이지만, 정적 타이핑(static typing)처럼 타입을 지정할 수 있도록 typing 모듈을 제공함.

---

## 1. Python의 동적 타이핑 vs 정적 타이핑

- 동적 타이핑 (Dynamic Typing)
  - Python은 변수에 타입을 명시하지 않아도 자동으로 타입이 결정됨
  - 런타임(실행 중)에 변수 타입이 변경될 수 있음
  - 예제:
   
    ```python
    x = 10       # int
    x = "hello"  # str (동적으로 타입 변경 가능)
    ```

- 정적 타이핑 (Static Typing, Type Hinting 활용)
  - typing 모듈을 사용하여 변수 및 함수의 타입을 지정 가능
  - Python 인터프리터가 강제하지 않지만, IDE와 린터(Linter)가 타입 체크를 지원함
  - 예제:
  
    ```python
    from typing import List

    def sum_numbers(numbers: List[int]) -> int:
        return sum(numbers)

    result = sum_numbers([1, 2, 3])  # ✅ 올바른 사용
    print(result)  # 출력: 6
    ```

---

## 2. typing 모듈의 주요 타입과 예시

| 타입          | 설명               | 예제                                         |
|---------------|--------------------|----------------------------------------------|
| int           | 정수형            | x: int = 10                                  |
| float         | 실수형            | pi: float = 3.14                             |
| str           | 문자열            | name: str = "Alice"                          |
| bool          | 불리언            | flag: bool = True                            |
| List[T]       | 리스트 타입       | numbers: List[int] = [1, 2, 3]               |
| Tuple[T, ...] | 튜플 타입         | point: Tuple[int, int] = (2, 3)              |
| Dict[K, V]    | 딕셔너리 타입     | user: Dict[str, int] = {"age": 25}           |
| Set[T]        | 집합 타입         | ids: Set[int] = {1, 2, 3}                    |
| Optional[T]   | None 허용 타입    | value: Optional[int] = None                  |
| Union[T1, T2] | 여러 타입 허용    | data: Union[int, str] = "Hello"              |
| Any           | 모든 타입 허용    | x: Any = 100                                 |
| ClassVar[T]   | 클래스 변수       | CONSTANT: ClassVar[int] = 10                 |

---

### 주요 타입별 예시

#### 기본 타입: int, float, str, bool

```python
x: int = 10         # 정수형
pi: float = 3.14    # 실수형
name: str = "Alice" # 문자열
flag: bool = True   # 불리언
```

#### 리스트 타입: List[T]

```python
from typing import List
numbers: List[int] = [1, 2, 3]
```

#### 튜플 타입: Tuple[T, ...]

```python
from typing import Tuple
point: Tuple[int, int] = (2, 3)
```

#### 딕셔너리 타입: Dict[K, V]

```python
from typing import Dict
user: Dict[str, int] = {"age": 25}
```

#### 집합 타입: Set[T]

```python
from typing import Set
ids: Set[int] = {1, 2, 3}

```
#### None 허용 타입: Optional[T]

```python
from typing import Optional
value: Optional[int] = None
```

#### 여러 타입 허용: Union[T1, T2]

```python
from typing import Union
data: Union[int, str] = "Hello"
```

#### 모든 타입 허용: Any

```python
from typing import Any
x: Any = 100
```

#### 클래스 변수: ClassVar[T]

```python
from typing import ClassVar
class Example:
    CONSTANT: ClassVar[int] = 100  # 클래스 변수

    def __init__(self, value: int):
        self.value = value  # 인스턴스 변수

example = Example(10)
print(example.CONSTANT)  # 100
```

---

## 3. 왜 typing을 사용해야 할까?

Python에서 typing을 사용하면 다음과 같은 장점이 있음:

1. **가독성 향상**
   - 타입 힌트를 통해 함수나 변수가 어떤 타입을 다루는지 명확히 알 수 있음
   - 코드 읽기와 유지보수가 쉬워짐

2. **오류 예방**
   - 잘못된 타입을 함수에 전달하거나 변수에 저장하려는 실수를 방지할 수 있음
   - IDE와 린터(Linter)가 문제를 사전에 알려줌

3. **IDE 지원 강화**
   - 타입 힌트를 활용하면 IDE가 정확한 자동완성과 타입 정보를 제공함
   - 생산성을 크게 높일 수 있음

4. **복잡한 타입 처리**
   - 리스트, 딕셔너리, 클래스 변수 등 복잡한 타입을 명확히 정의할 수 있음
   - 예:
  
        ```python
        from typing import Dict, List
        user_data: Dict[str, List[int]] = {"scores": [90, 85, 88]}
        ```

5. **협업 효율성 증가**
   - 타입 힌트를 사용하면 코드 작성 의도를 명확히 전달할 수 있음
   - 팀원 간의 커뮤니케이션 비용을 줄여줌

---

## 4. 결론

Python의 typing은 동적 타이핑의 유연함을 유지하면서도 정적 타이핑의 장점을 도입할 수 있는 강력한 도구임. 

이를 활용하면 코드 가독성과 안정성이 크게 향상되며, 특히 대규모 프로젝트에서 중요한 역할을 함.
