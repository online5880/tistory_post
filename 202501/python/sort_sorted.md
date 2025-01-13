
# Python 정렬 메서드 비교: `sort`와 `sorted`

## 1. 차이점
### `sorted`
- **새로운 객체 반환**: 기존 리스트를 변경하지 않고, 정렬된 새로운 리스트를 반환함.
- 사용 방식: `sorted(iterable, *, key=None, reverse=False)`
- 원본 데이터는 변경되지 않음.

### `sort`
- **원본 리스트 직접 변경**: 리스트 객체에서 메서드로 호출하며, 리스트를 제자리에서 정렬함.
- 사용 방식: `list.sort(*, key=None, reverse=False)`
- 반환값: `None` (작업 후 리스트를 반환하지 않음).

---

## 2. 예제 코드
```python
# 리스트 데이터 준비
f_list = ['orange', 'apple', 'mango', 'papaya', 'lemon', 'strawberry', 'coconut']

# sorted 예제
print('sorted - ', sorted(f_list))  # 알파벳 순으로 정렬
print('sorted - ', sorted(f_list, reverse=True))  # 역순 정렬
print('sorted - ', sorted(f_list, key=len))  # 문자열 길이 기준 정렬
print('sorted - ', sorted(f_list, key=lambda x: x[-1]))  # 마지막 문자 기준 정렬
print('sorted - ', sorted(f_list, key=lambda x: x[-1], reverse=True))  # 마지막 문자 기준 역순 정렬
print('original list - ', f_list)  # 원본 리스트 확인 (변경되지 않음)

# sort 예제
f_list.sort()  # 알파벳 순 정렬
print('sort - ', f_list)
f_list.sort(reverse=True)  # 역순 정렬
print('sort - ', f_list)
f_list.sort(key=len)  # 문자열 길이 기준 정렬
print('sort - ', f_list)
f_list.sort(key=lambda x: x[-1])  # 마지막 문자 기준 정렬
print('sort - ', f_list)
f_list.sort(key=lambda x: x[-1], reverse=True)  # 마지막 문자 기준 역순 정렬
print('sort - ', f_list)
```

---

## 3. 주요 파라미터
- **`key`**: 정렬 기준을 지정하는 함수.
  - 예: `key=len` → 문자열 길이로 정렬
  - 예: `key=lambda x: x[-1]` → 마지막 문자를 기준으로 정렬
- **`reverse`**: 정렬 순서를 역순으로 지정.
  - `reverse=True`: 내림차순 정렬
  - 기본값: `reverse=False`

---

## 4. 주요 차이 요약

| 특징            | `sort`                          | `sorted`                           |
|-----------------|--------------------------------|------------------------------------|
| 사용 방식         | `리스트명.sort()`              | `sorted(리스트)`                  |
| 반환값           | `None`                         | 정렬된 새로운 리스트 반환           |
| 원본 데이터 변경 | 변경됨                          | 변경되지 않음                      |
| 데이터 유형       | 리스트에서만 사용 가능           | 모든 iterable 사용 가능 (리스트, 튜플 등) |

---

## 5. 실행 결과 예시
```python
# sorted 결과
sorted -  ['apple', 'coconut', 'lemon', 'mango', 'orange', 'papaya', 'strawberry']
sorted -  ['strawberry', 'papaya', 'orange', 'mango', 'lemon', 'coconut', 'apple']
sorted -  ['mango', 'lemon', 'apple', 'orange', 'papaya', 'coconut', 'strawberry']
sorted -  ['papaya', 'mango', 'orange', 'apple', 'strawberry', 'lemon', 'coconut']
sorted -  ['coconut', 'lemon', 'strawberry', 'apple', 'orange', 'mango', 'papaya']
original list -  ['orange', 'apple', 'mango', 'papaya', 'lemon', 'strawberry', 'coconut']

# sort 결과
sort -  ['apple', 'coconut', 'lemon', 'mango', 'orange', 'papaya', 'strawberry']
sort -  ['strawberry', 'papaya', 'orange', 'mango', 'lemon', 'coconut', 'apple']
sort -  ['mango', 'lemon', 'apple', 'orange', 'papaya', 'coconut', 'strawberry']
sort -  ['papaya', 'mango', 'orange', 'apple', 'strawberry', 'lemon', 'coconut']
sort -  ['coconut', 'lemon', 'strawberry', 'apple', 'orange', 'mango', 'papaya']
```

---

## 6. 주의점
1. **`sort`는 원본 리스트를 변경**하므로, 원본 데이터를 유지해야 할 경우 `sorted`를 사용하는 것이 적합함.
2. **`key` 함수 활용**: 정렬 기준을 자유롭게 설정 가능.
3. **`reverse` 옵션**: 정렬 순서를 손쉽게 바꿀 수 있음.