
# Linux vi 편집기 기본 명령어

`vi`는 Linux에서 많이 사용되는 텍스트 편집기 중 하나로, 두 가지 모드(INSERT 모드와 COMMAND 모드)를 활용하여 다양한 작업을 수행할 수 있음.

## 모드 설명
- **INSERT 모드 (`i`)**: 문자를 입력할 수 있는 모드.
- **COMMAND 모드 (`esc`)**: 복사/붙여넣기, 파일 저장/종료 등 다양한 작업을 수행할 수 있는 모드.

## COMMAND 모드에서의 주요 명령어
### 파일 작업
- **종료 (quit)**: `q` 입력 후 Enter
- **저장 (write) 및 종료**: `wq` 입력 후 Enter
- **저장 및 강제 종료**: `wq!` 입력 후 Enter

### 편집 작업
- **줄 삭제 (delete)**: `dd`
- **복사 (copy)**: `yy`
- **붙여넣기 (paste)**: `p`
- **되돌리기 (undo)**: `u`

### 뷰 설정
- **라인 번호 보기**: `:set number`

### 텍스트 탐색 및 수정
- **텍스트 찾기**: `/검색어` 입력 후 `n`으로 다음 검색 결과 탐색
- **텍스트 대체**: `:%s/원본/대체/g`

## 사용 예시
아래는 vi 편집기에서 간단한 작업 예시:
```bash
# vi 편집기 실행
vi example.txt

# INSERT 모드로 전환 후 텍스트 입력
i
Hello, World!  # 입력 후 ESC로 COMMAND 모드로 전환

# 줄 삭제
dd  # 현재 줄 삭제

# 텍스트 복사 및 붙여넣기
yy  # 현재 줄 복사
p   # 복사한 줄 붙여넣기

# 텍스트 찾기
/SearchTerm  # 'SearchTerm'을 검색
n            # 다음 결과로 이동

# 텍스트 대체
:%s/old/new/g  # 'old'를 'new'로 대체
```

---

### 참고 이미지
![vi 편집기 모드](https://upload.wikimedia.org/wikipedia/commons/7/75/Vim_modes.svg)
*이미지 출처: [Wikimedia Commons](https://commons.wikimedia.org)*
