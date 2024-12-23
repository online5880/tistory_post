
# 리눅스 파일시스템 기본 명령어

리눅스 시스템에서 파일과 디렉터리를 효율적으로 관리하기 위해 자주 사용되는 명령어들을 정리함.

---

## 1. pwd
- **Print Working Directory**
- 현재 위치를 확인하는 명령어
```bash
pwd
```
### 실행 예시
```bash
/home/user
```

---

## 2. mkdir
- 디렉터리를 만드는 명령어
```bash
mkdir [디렉터리 이름]
```
### 실행 예시
```bash
mkdir new_folder
```

---

## 3. cd
- 디렉터리를 이동하는 명령어
```bash
cd [이동할 디렉터리]
```
### 실행 예시
```bash
cd new_folder
```

---

## 4. ls
- 디렉터리의 파일 리스트를 조회하는 명령어
```bash
ls
```
### 실행 예시
```bash
file1.txt  file2.txt  folder1
```

---

## 5. tree
- 디렉터리/파일의 구조를 확인하는 명령어

### 사용 예시
1. 일반 조회:
    ```bash
    tree
    ```
    #### 출력 예시
    ```
    .
    ├── file1.txt
    ├── file2.txt
    └── folder1
        └── file3.txt
    ```
2. 디렉터리만 조회:
    ```bash
    tree -d
    ```
    #### 출력 예시
    ```
    .
    └── folder1
    ```

---

## 6. cp
- 디렉터리/파일을 복사하는 명령어

### 사용 예시
1. 파일 복사:
    ```bash
    cp file1.txt /path/to/destination/
    ```
2. 다른 이름으로 복사:
    ```bash
    cp file1.txt new_file.txt
    ```
3. 디렉터리 복사:
    ```bash
    cp -r folder1 /path/to/destination/
    ```
4. 현재 디렉터리로 복사:
    ```bash
    cp -r /source/folder .
    ```

---

## 7. mv
- 디렉터리/파일을 이동하는 명령어

### 사용 예시
1. 파일/디렉터리 이동:
    ```bash
    mv file1.txt /path/to/destination/
    ```
2. 이름 변경:
    ```bash
    mv old_name.txt new_name.txt
    ```

---

## 8. find
- 파일/디렉터리를 찾는 명령어
```bash
find [찾을 위치] -name [파일/디렉터리명]
```
### 실행 예시
```bash
find . -name "file1.txt"
```
#### 출력 예시
```
./folder1/file1.txt
```

---

## 9. rm
- 디렉터리/파일을 삭제하는 명령어

### 사용 예시
1. 파일 삭제:
    ```bash
    rm file1.txt
    ```
2. 디렉터리 삭제:
    ```bash
    rm -r folder1
    ```
3. 강제 삭제(주의):
    ```bash
    rm -rf folder1
    ```

---

### 추가 참고사항
- 명령어 사용 시 실수로 데이터를 잃지 않도록 조심해야 함.
- 특히 `rm -rf`는 강력한 삭제 명령이므로 주의하여 사용해야 함.

