## **[SSH] VSCode Remote SSH Permission error**

---

<img src="https://github.com/online5880/tistory_post/blob/main/202412/project/ssh_permission.png?raw=true" width="75%" height="50%"/>

### **AS-IS**
- **문제상황 인지**  
  VSCode Remote SSH를 통해 원격 서버에 연결된 상태에서 `docker-compose.yml` 파일 저장 시 권한 오류 발생  
- **해결하려고 하는 문제**  
  파일 저장 권한 문제 해결  
- **만들고 싶은 기능**  
  원격 서버의 파일을 수정 후 정상적으로 저장  

---

### **Challenge**
- **문제해결을 위해 고민한 내용**  
  - 현재 사용자 권한 확인 (`ls -l` 또는 `whoami`)  
  - 파일과 상위 디렉토리 권한 확인  
  - 파일 소유권과 그룹 권한 변경 시도  
  - 파일 수정 권한 부여를 위해 `chmod` 명령어 사용  
- **참고 블로그**  
  [VS Code에서 Remote SSH를 이용해 원격에 있는 파일을 저장할 때 permission 문제](https://velog.io/@kjhxxxx/VS-Code%EC%97%90%EC%84%9C-Remote-SSH%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4-%EC%9B%90%EA%B2%A9%EC%97%90-%EC%9E%88%EB%8A%94-%ED%8C%8C%EC%9D%BC%EC%9D%84-%EC%A0%80%EC%9E%A5%ED%95%A0-%EB%95%8C-permission-%EB%AC%B8%EC%A0%9C)  

- **어떻게 기술적으로 해결했는지**  
  1. 터미널에서 권한 확인  
     ```bash
     ls -l /home/ubuntu/nginx/docker-compose.yml
     ```
  2. 소유권 변경 (필요시)  
     ```bash
     sudo chown $USER /home/ubuntu/nginx/docker-compose.yml
     ```
  3. 권한 부여  
     ```bash
     sudo chmod 644 /home/ubuntu/nginx/docker-compose.yml
     ```

---

### **TO-BE**
- **아웃풋 (결과)**  
  VSCode Remote SSH를 통해 파일을 정상적으로 저장할 수 있음.  
  추가적으로 권한 이슈가 재발하지 않도록 설정된 사용자 권한 유지 확인.
