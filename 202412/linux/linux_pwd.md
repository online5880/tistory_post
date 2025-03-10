

# 리눅스 `pwd` 명령어 사용법

`pwd`는 **현재 작업 중인 디렉토리의 절대 경로를 출력**하는 명령어임. Linux 시스템에서 파일이나 디렉토리를 다룰 때 현재 위치를 확인하는 데 유용함.

---

## 1. 기본 사용법
```bash
pwd [옵션]
```

### 예시
1. 현재 디렉토리 확인:
   ```bash
   pwd
   ```
   출력 예시: `/home/user/documents`

---

## 2. 주요 옵션
| 옵션         | 설명                                                      |
|--------------|----------------------------------------------------------|
| `-L`         | 심볼릭 링크를 따라가서 경로를 출력 (기본값)                  |
| `-P`         | 실제 경로(심볼릭 링크를 해제한 경로)를 출력                   |

### 예시
1. 심볼릭 링크를 따라 경로 출력 (`-L` 옵션, 기본 동작):
   ```bash
   pwd -L
   ```
   출력 예시: `/home/user/symlink_dir`

2. 실제 경로 출력 (`-P` 옵션):
   ```bash
   pwd -P
   ```
   출력 예시: `/home/user/real_dir`

---

## 3. 활용 예제
1. 현재 작업 중인 디렉토리 확인:
   ```bash
   pwd
   ```
   → 절대 경로를 출력.

2. 심볼릭 링크가 포함된 경로를 해제하고 실제 경로 확인:
   ```bash
   pwd -P
   ```
   → 심볼릭 링크를 따라가지 않고 실제 경로를 출력.

---

## 4. 주의사항
- `pwd`는 `bash`와 같은 셸에서 기본 제공하는 명령어로, 외부 명령어(`/bin/pwd`)와 결과가 약간 다를 수 있음.
- `-L` 옵션과 `-P` 옵션은 심볼릭 링크를 다룰 때 명확한 차이를 보임.

---

## 5. 요약
- `pwd`는 현재 디렉토리의 절대 경로를 확인하는 명령어.
- `-L`과 `-P` 옵션을 활용하면 심볼릭 링크를 포함한 경로나 실제 경로를 구분하여 확인 가능.
