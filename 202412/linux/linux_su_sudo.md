
# su와 sudo 명령어의 차이와 사용법

리눅스와 유닉스 시스템에서는 권한 관리가 매우 중요함. 

`su`와 `sudo`는 시스템 관리자가 권한을 변경하거나, 제한된 작업을 수행할 때 사용하는 대표적인 명령어임. 

이 글에서는 두 명령어의 차이와 사용법을 설명함.

---

## su 명령어란?

`su`는 **substitute user** 또는 **switch user**의 약자로, 현재 사용자에서 다른 사용자로 전환하는 명령어임. 기본적으로 `root` 사용자로 전환할 때 많이 사용됨.

### 사용법
```bash
su [사용자명]
```

### 주요 예시
1. **root 사용자로 전환**:
   ```bash
   su
   ```
   비밀번호를 입력하면 root 계정으로 전환됨.

2. **특정 사용자로 전환**:
   ```bash
   su user1
   ```
   `user1` 계정으로 전환함.

3. **root 계정으로 로그인 후 특정 명령 실행**:
   ```bash
   su -c "command"
   ```
   예: 
   ```bash
   su -c "apt update"
   ```

### 장점
- 사용자가 완전히 다른 계정으로 전환할 수 있음.
- root 계정으로 전환 시 시스템 전체에 대한 권한이 부여됨.

### 단점
- root 계정의 비밀번호를 알아야 함.
- 보안상 위험이 존재하며, root로 완전히 전환되면 실수로 시스템 전체를 손상시킬 수 있음.

---

## sudo 명령어란?

`sudo`는 **superuser do**의 약자로, 특정 명령어를 **관리자 권한**으로 실행할 수 있도록 하는 명령어임. `sudo`는 사용자가 root 비밀번호가 아닌 자신의 비밀번호를 사용해 권한을 획득함.

### 사용법
```bash
sudo [명령어]
```

### 주요 예시
1. **특정 명령을 관리자 권한으로 실행**:
   ```bash
   sudo apt update
   ```
   현재 사용자 계정의 비밀번호를 입력하면 해당 명령이 관리자 권한으로 실행됨.

2. **특정 사용자가 관리자 권한 명령 실행 가능 여부 확인**:
   ```bash
   sudo -l
   ```
   사용자가 실행할 수 있는 명령어 리스트를 보여줌.

3. **root 셸 실행**:
   ```bash
   sudo -i
   ```

### 장점
- 특정 명령어만 root 권한으로 실행하므로 보안성이 높음.
- root 계정 비밀번호를 알 필요가 없음.
- `/etc/sudoers` 파일을 통해 사용자의 권한을 세부적으로 설정 가능.

### 단점
- 설정이 필요하며, 사용자가 `sudo` 권한을 가지지 않을 경우 사용할 수 없음.
- 비밀번호 입력 과정이 필요해 다소 번거로울 수 있음.

---

## su와 sudo의 차이점

| 특징          | su                                               | sudo                                         |
|---------------|--------------------------------------------------|---------------------------------------------|
| 의미          | 사용자 계정 전환                                  | 특정 명령어를 관리자 권한으로 실행           |
| 사용 권한     | root 계정 비밀번호 필요                            | 현재 사용자 비밀번호 필요                   |
| 권한 범위     | 전체 셸 또는 사용자 환경                          | 명령어 단위로 제한                         |
| 보안성        | 보안 취약 가능성 있음                             | 보안성이 더 높음                           |
| 설정 필요 여부| 별도 설정 불필요                                  | sudoers 파일 설정 필요                      |

---

## su와 sudo의 적합한 사용 시점

1. **su를 사용하는 경우**:
   - 관리자 계정으로 장시간 작업을 해야 하는 경우.
   - 다른 사용자 계정으로 전환이 필요한 경우.

2. **sudo를 사용하는 경우**:
   - 특정 명령만 관리자 권한으로 실행해야 하는 경우.
   - root 비밀번호를 공유하지 않고 권한을 제한적으로 부여하고자 할 때.

---

## 결론

`su`와 `sudo`는 각기 다른 목적과 장점을 가지고 있음. `su`는 전체 계정 전환이 필요할 때 적합하며, `sudo`는 특정 작업만 관리자 권한으로 실행하고자 할 때 유용함. 일반적으로 보안성 측면에서는 `sudo`가 더 우수하며, 시스템 관리자는 `sudo`를 기본으로 사용하고 필요한 경우에만 `su`를 사용하는 것이 권장됨.
