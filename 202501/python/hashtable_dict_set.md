
# Python의 해시테이블(Hashtable) 활용: Dict와 Set

Python에서 해시테이블은 적은 리소스로 대량의 데이터를 효율적으로 관리할 수 있는 핵심 구조임 

특히 `Dict`와 `Set`은 데이터 중복을 허용하지 않고 빠른 검색을 가능하게 하여, 다양한 프로그래밍 문제에서 중요한 역할을 함

---

## 1. Dictionary와 Set의 기본 특징

### Dictionary (`Dict`)
- 키-값 쌍으로 데이터를 관리
- **Key는 중복 불가**, **Value는 중복 허용**
- 삽입 순서를 유지

### Set
- 데이터의 중복을 허용하지 않음
- 순서가 없으며, 해싱을 기반으로 빠른 연산 가능
- 추가적으로 `frozenset`을 통해 불변(immutable)한 집합을 생성 가능

---

## 2. 불변 Dictionary (`Immutable Dict`)

Python의 `MappingProxyType`을 사용하면, 읽기 전용(불변) `Dict`를 생성할 수 있음 이는 데이터 변경을 방지하여 보안성과 무결성을 높이는 데 유용함

```python
from types import MappingProxyType

# 수정 가능한 딕셔너리
d = {'key1': 'value1'}

# 읽기 전용 딕셔너리 생성
d_frozen = MappingProxyType(d)

# 출력 및 ID 비교
print(d, id(d))
print(d_frozen, id(d_frozen))

# 수정 불가 (에러 발생)
# d_frozen['key2'] = 'value2'

# 원본 딕셔너리는 수정 가능
d['key2'] = 'value2'
print(d)
```

출력 결과:
```
{'key1': 'value1'} 140589740123456
{'key1': 'value1'} 140589740123789
{'key1': 'value1', 'key2': 'value2'}
```

---

## 3. Set의 다양한 활용

### Set 기본 생성과 특징
```python
s1 = {'Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'}
s2 = set(['Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'])
s3 = {3}
s4 = set()  # 빈 집합
s5 = frozenset({'Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'})

s1.add('Melon')  # 추가 가능
print(s1)

# frozenset은 불변 (추가 불가)
# s5.add('Melon')  # 에러 발생

print(s1, type(s1))
print(s2, type(s2))
print(s3, type(s3))
print(s4, type(s4))
print(s5, type(s5))
```

출력 결과:
```
{'Apple', 'Orange', 'Kiwi', 'Melon'}
{'Apple', 'Orange', 'Kiwi', 'Melon'} <class 'set'>
{'Apple', 'Orange', 'Kiwi'} <class 'set'>
{3} <class 'set'>
set() <class 'set'>
frozenset({'Apple', 'Orange', 'Kiwi'}) <class 'frozenset'>
```

---

## 4. 선언 최적화

`Set` 생성 방법에 따라 Python 인터프리터가 다르게 처리됨 `dis` 모듈을 통해 바이트코드를 확인하면 성능 최적화 힌트를 얻을 수 있음

```python
from dis import dis

print('-'*50)
print(dis('{10}'))  # Set 리터럴
print('-'*50)
print(dis('set([10])'))  # Set 생성자
```

출력 결과:
```
  1           0 LOAD_CONST               0 (10)
              2 BUILD_SET                1
              4 RETURN_VALUE
--------------------------------------------------
  1           0 LOAD_NAME                0 (set)
              2 LOAD_CONST               0 (10)
              4 BUILD_LIST               1
              6 CALL_FUNCTION            1
              8 RETURN_VALUE
```
`Set` 리터럴이 더 효율적임을 알 수 있음

---

## 5. 지능형 집합 (Comprehending Set)

지능형 집합을 사용하면 한 줄의 코드로 다양한 데이터 처리 작업을 수행할 수 있음

```python
from unicodedata import name

# 유니코드 문자 이름 집합 생성
unicode_set = {name(chr(i), '') for i in range(0, 256)}
print(unicode_set)
```

출력 결과:
```
{'NULL', 'START OF HEADING', 'LATIN SMALL LETTER A', ... }
```

---

## 정리 및 활용 팁

1. **Dictionary**: 데이터 저장과 조회가 빈번히 발생하는 상황에 유용
2. **Set**: 데이터 중복 제거 및 교집합, 차집합 등 수학적 연산에 적합
3. **Immutable 구조**: 데이터 무결성을 보장해야 하는 환경에서 활용
