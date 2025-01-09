# Python에서의 Instance Method, Class Method, Static Method 이해하기

Python은 객체지향 프로그래밍 언어로, 메서드를 통해 객체와 클래스 간의 다양한 작업을 수행할 수 있음. 

이 글에서는 **Instance Method**, **Class Method**, **Static Method**의 차이를 이해하고, 언제 어떤 것을 사용해야 하는지 살펴봄.

## 1. Instance Method
Instance Method는 클래스의 인스턴스에 바인딩되어 있으며, 인스턴스 데이터를 읽거나 수정할 수 있음. 첫 번째 매개변수로 **`self`**를 사용하며, 이는 호출한 인스턴스 자체를 참조함.

### 특징
- 클래스의 인스턴스 데이터에 접근 가능
- `self`를 통해 인스턴스 속성 및 다른 메서드에 접근 가능

### 예제
```python
class MyClass:
    def __init__(self, value):
        self.value = value
    
    def instance_method(self):
        return f"Instance method called, value is {self.value}"

obj = MyClass(10)
print(obj.instance_method())  # 출력: Instance method called, value is 10
```

---

## 2. Class Method
Class Method는 클래스 자체를 첫 번째 매개변수로 받음. 이는 클래스 레벨에서 작동하며, 클래스 변수에 접근하거나 클래스를 수정하는 데 주로 사용됨. **`@classmethod` 데코레이터**로 정의함.

### 특징
- 첫 번째 매개변수로 **`cls`**를 받아 클래스 자체를 참조
- 클래스 변수 및 클래스 메서드에 접근 가능

### 예제
```python
class MyClass:
    class_variable = "Class Level Variable"

    @classmethod
    def class_method(cls):
        return f"Class method called, class_variable is {cls.class_variable}"

print(MyClass.class_method())  # 출력: Class method called, class_variable is Class Level Variable
```

---

## 3. Static Method
Static Method는 클래스나 인스턴스에 의존하지 않으며, 독립적으로 동작함. **`@staticmethod` 데코레이터**로 정의함. 클래스나 인스턴스 데이터를 전혀 사용하지 않을 때 사용함.

### 특징
- 클래스 및 인스턴스와 독립적으로 동작
- 유틸리티 함수처럼 사용 가능

### 예제
```python
class MyClass:
    @staticmethod
    def static_method(x, y):
        return f"Static method called, result is {x + y}"

print(MyClass.static_method(5, 10))  # 출력: Static method called, result is 15
```

---

## 주요 차이점 요약

| 유형            | 첫 번째 매개변수 | 접근 가능 대상            | 용도                     |
|----------------|----------------|------------------------|------------------------|
| Instance Method | `self`         | 인스턴스 속성 및 메서드    | 인스턴스 데이터를 조작하거나 처리할 때 |
| Class Method    | `cls`          | 클래스 변수 및 클래스 메서드 | 클래스 레벨 작업을 수행할 때       |
| Static Method   | 없음           | 없음                     | 독립적인 작업이나 유틸리티 함수 작성 시 |

---

## 언제 어떤 메서드를 사용해야 할까?
1. **Instance Method**:
   - 인스턴스 데이터를 조작하거나 읽어야 하는 경우.
   - 예: 사용자 정보 업데이트, 특정 객체 상태 반환.

2. **Class Method**:
   - 클래스 레벨의 작업(클래스 변수 조작 등)을 수행해야 하는 경우.
   - 예: 새로운 객체를 생성하는 팩토리 메서드 구현.

3. **Static Method**:
   - 클래스나 인스턴스 데이터와 무관한 작업(유틸리티 함수)을 수행해야 하는 경우.
   - 예: 데이터 포맷팅, 단순 계산 함수.

---

## 마무리
Python의 메서드 유형은 목적에 맞게 사용해야 코드의 가독성과 유지보수성을 높일 수 있음. **Instance Method**, **Class Method**, **Static Method** 각각의 역할과 특징을 이해하고 적절히 활용해 보자.
