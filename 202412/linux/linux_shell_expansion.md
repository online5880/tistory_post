# 리눅스 쉘 확장(Expansion) 정리

리눅스 쉘에서 **확장(Expansion)** 은 입력된 명령어를 특정 규칙에 따라 변환하여 실행 전에 해석하는 강력한 기능임. 

여기에서는 와일드카드, 중괄호 확장, 산술 확장, 명령어 치환 등의 다양한 확장 기능을 정리함.

---

## 1. 와일드카드 확장
### 1.1 `*` 와일드카드
- 모든 파일이나 디렉터리와 일치.
- 예시:
  ```bash
  echo *.txt
  echo p*
  echo -l **at**
  ```

### 1.2 `?` 와일드카드
- 하나의 문자와만 일치.
- 예시:
  ```bash
  echo app?.css
  echo *.???
  ```

### 1.3 범위 와일드카드 (`[]`)
- 특정 문자 범위와 일치.
- 예시:
  ```bash
  ls [A-F]*
  echo app[132].css
  echo app[1-3].css
  echo [A-Z]*
  echo [A-H]*[ps]
  ```

### 1.4 범위 부정 (`[^]`)
- 특정 문자 범위와 일치하지 않는 항목을 선택.
- 예시:
  ```bash
  ls [^a]*
  ```

---

## 2. 중괄호 확장
### 2.1 기본 중괄호 확장
- 여러 선택지를 생성.
- 예시:
  ```bash
  touch page{1,2,3}.txt
  echo Dec_{Mon,Thu,Wed,Thu,Fri}_Planner.txt
  echo {Mon,Thu,Wed,Thu,Fri}_{AM,PM}.txt
  ```

### 2.2 숫자 및 문자 범위
- 숫자 범위와 문자 범위를 사용하여 반복적으로 생성.
- 예시:
  ```bash
  echo journal{1..365}.txt
  echo {2..10..1}
  echo {2..10..3}
  echo {a..e}
  ```

### 2.3 디렉터리 구조 생성
- 중첩된 디렉터리 생성.
- 예시:
  ```bash
  mkdir -p {Mon,Tue,Wed,Thu,Fri,Sat,Sun}/{Breakfast,Lunch,Dinner}
  ```

### 2.4 중첩 중괄호
- 중괄호를 중첩하여 복잡한 확장 수행.
- 예시:
  ```bash
  echo {x,y{1..5},z}
  echo {Mon,Tue{1..10},Weds}
  ```

---

## 3. 산술 확장
### 개념
- `$((expression))`을 사용하여 산술 계산을 수행.
- 예시:
  ```bash
  echo $((10+3))
  echo $((10*3))
  echo $((10/3))
  echo $((10**3))
  echo $((10%3))
  ```

---

## 4. 큰따옴표와 작은따옴표
### 4.1 큰따옴표 (`"`)
- 특수 문자를 해석.
- 예시:
  ```bash
  echo “$((2+2)) is 4”  # 출력: 4 is 4
  ```

### 4.2 작은따옴표 (`'`)
- 특수 문자를 무시.
- 예시:
  ```bash
  echo '$((2+2)) is 4'  # 출력: $((2+2)) is 4
  ```

---

## 5. 명령어 치환 (Command Substitution)
### 개념
- 명령어의 출력을 확장하여 명령어 내에 삽입.
- `$(command)` 또는 백틱(```command```)을 사용.
- 예시:
  ```bash
  echo Hello there $(whoami)
  echo today is `date`
  ```

---

### 추천 해시태그
`#리눅스` `#쉘스크립트` `#확장` `#와일드카드` `#중괄호확장` `#산술확장` `#명령어치환` `#리눅스기초` `#터미널` `#리눅스팁`
