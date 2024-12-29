
# 리눅스 `grep` 명령어

리눅스에서 파일의 내용을 검색하거나 특정 패턴을 찾기 위해 사용하는 강력한 도구가 바로 `grep`임. 

`grep`은 정규식을 활용해 복잡한 패턴도 쉽게 매칭 가능하며, 다양한 옵션과 함께 강력한 검색 기능을 제공함.

---

## 1. 기본 사용법
```bash
grep PATTERN FILE
```

### 주요 옵션
- **대소문자 무시**  
  ```bash
  grep -i pattern file
  ```

- **단어 검색**  
  ```bash
  grep -w pattern file
  ```

- **재귀적 검색**  
  디렉토리 내 모든 파일을 검색:  
  ```bash
  grep -r "apple"
  ```

  대소문자 무시 및 재귀적 검색:  
  ```bash
  grep -ri "korea"
  ```

---

## 2. 다양한 옵션 사용 예시
- **패턴 개수 출력**  
  ```bash
  grep "myself" SongOfMyself.txt -ic
  ```

- **단어 단위 검색과 대소문자 무시**  
  ```bash
  grep "i" SongOfMyself.txt -wi
  ```

- **검색 결과와 바로 다음 줄 출력**  
  ```bash
  grep "wagon" SongOfMyself.txt -iA1
  ```

- **검색 결과와 앞/뒤 줄 출력**  
  ```bash
  grep "wagon" SongOfMyself.txt -A1 -B1
  ```

- **줄 번호와 함께 검색**  
  ```bash
  grep "6" SongOfMyself.txt -wn
  ```

- **검색 결과와 한 줄 앞뒤 포함**  
  ```bash
  grep "wagon" SongOfMyself.txt -nC1
  ```

---

## 3. `grep`과 정규 표현식
정규 표현식을 사용하면 더욱 세밀한 검색이 가능함.

### 예시
- **특정 패턴 검색**  
  ```bash
  grep 'p.....' SongOfMyself.txt  # p로 시작하는 6글자 단어
  ```

- **문장의 시작 검색**  
  ```bash
  grep "^I" SongOfMyself.txt  # 'I'로 시작하는 문장
  ```

- **문장의 끝 검색**  
  ```bash
  grep ")$" SongOfMyself.txt  # ')'로 끝나는 문장
  ```

- **범위 내 숫자 검색**  
  ```bash
  grep "2[1-6]" SongOfMyself.txt  # 21~26 포함
  ```

- **범위 밖 숫자 검색**  
  ```bash
  grep "2[^1-6]" SongOfMyself.txt  # 21~26 제외
  ```

---

## 4. `grep`과 확장 정규식 (-E 옵션)
확장된 정규식을 사용하면 더욱 복잡한 패턴 매칭이 가능함.

### 예시
- **단수/복수 검색**  
  ```bash
  grep "birds?" -wE SongOfMyself.txt  # bird 또는 birds
  ```

- **모음 두 개 연속**  
  ```bash
  grep "[aeiou]{2}" -E SongOfMyself.txt
  ```

- **모음 2~4개 연속**  
  ```bash
  grep "[aeiou]{2,4}" -E SongOfMyself.txt
  ```

---

## 5. `grep`과 파이프 활용
파이프를 사용해 다른 명령어와 결합 가능.

### 예시
- **`ps` 출력에서 특정 프로세스 검색**  
  ```bash
  ps -aux | grep "sound" -i
  ```

- **`man` 페이지에서 특정 키워드 검색**  
  ```bash
  man grep | grep "count"
  ```

- **`find` 명령과 결합**  
  ```bash
  find / -name "*.txt" ! -empty -type f -exec grep -l "grass" '{}' ';'
  ```

---

## 마무리
`grep`은 리눅스에서 파일 검색과 분석을 위한 필수 도구로, 다양한 옵션과 정규식을 조합하면 거의 모든 패턴을 효과적으로 찾을 수 있음.