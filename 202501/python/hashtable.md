
# 해시 테이블 (Hash Table)란?

해시 테이블은 **Key-Value** 형태로 데이터를 저장하는 자료구조로, **Key 값을 해싱 함수(Hashing Function)**를 통해 계산된 해시 주소에 매핑하여 데이터에 빠르게 접근할 수 있도록 함. 

Python에서는 `dict`와 `set`이 대표적으로 해시 테이블을 기반으로 구현된 자료구조임.

---

## 파이썬 `dict`의 특징

1. **해시 값을 기반으로 한 접근성**  
   키 값의 해싱 결과에 따라 특정 메모리 주소로 바로 접근이 가능하므로 검색 속도가 매우 빠름.

2. **Mutable, Dynamic**  
   `dict`는 가변적이며, 필요한 만큼 동적으로 확장할 수 있음.

3. **Key의 조건**  
   - `Key`는 해시 가능(immutable)해야 함. 
   - 해시 가능 여부는 `hash()` 함수로 확인 가능.
   - 예를 들어, 튜플은 요소가 모두 immutable한 경우에만 해시 가능.

---

### `dict`의 내부 구조 확인

파이썬 내장 네임스페이스에서 사용되는 `dict`의 구조를 확인할 수 있음.

```python
print(__builtins__.__dict__)
```

---

## 해시 가능 여부 확인

튜플과 리스트의 해시 가능 여부를 비교해보자.

```python
t1 = (10, 20, (30, 40, 50))  # 해시 가능
t2 = (10, 20, [30, 40, 50])  # 해시 불가능 (리스트 포함)

print(hash(t1))  # 출력 가능
# print(hash(t2))  # TypeError 발생
```

튜플 `t1`은 모든 요소가 immutable하여 해시 가능하지만, `t2`는 리스트를 포함하고 있어 해시 불가능.

---

## `dict.setdefault()`의 활용

`setdefault()`는 딕셔너리에 키-값 쌍을 추가하거나 키가 이미 존재하는 경우 기존 값을 유지하며 동작함. 이를 활용하면 코드가 간결해짐.

### 예제: 동일한 키를 가지는 데이터를 딕셔너리로 그룹화

#### 일반적인 방식
```python
source = (('k1', 'val1'),
          ('k1', 'val2'),
          ('k2', 'val3'),
          ('k2', 'val4'),
          ('k2', 'val5'))

new_dict1 = {}
for k, v in source:
    if k in new_dict1:
        new_dict1[k].append(v)
    else:
        new_dict1[k] = [v]

print(new_dict1)
# {'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}
```

#### `setdefault()`를 활용한 방식
```python
new_dict2 = {}
for k, v in source:
    new_dict2.setdefault(k, []).append(v)

print(new_dict2)
# {'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}
```

`setdefault()`를 사용하면 코드가 훨씬 간결하고 가독성이 좋아짐.

---

### 주의점: Dictionary Comprehension의 한계

```python
new_dict3 = {k: v for k, v in source}
print(new_dict3)
# {'k1': 'val2', 'k2': 'val5'}
```

`dict`는 중복된 키를 허용하지 않으므로 마지막 값만 저장됨. 따라서 여러 값을 그룹화하려면 `setdefault()`와 같은 방법을 활용해야 함.

---

## 결론

- 해시 테이블은 빠른 데이터 접근 속도를 제공하며, 파이썬의 `dict`는 이를 활용한 강력한 자료구조임.
- `setdefault()`를 활용하면 중복 키의 데이터를 간편하게 처리 가능.
- `Key`는 해시 가능해야 하며, `hash()` 함수로 이를 확인할 수 있음.