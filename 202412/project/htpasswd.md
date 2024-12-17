## AS-IS
현재 상황에서 인식한 문제점과 해결하고자 하는 내용입니다.

1. **문제상황 인지**  
   - **EC2**에서 Docker와 Nginx를 통해 **MLflow 서버**를 실행하고 있음.  
   - MLflow 서버에 별도의 인증 절차가 없어 **누구나 접속 가능한 보안 문제**가 발생.  
   - 보안 그룹 설정은 있지만 근본적으로 접근 통제를 위한 추가적인 보안이 필요함.

---

## Challenge
문제를 해결하기 위해 고민하고 기술적으로 접근한 과정입니다.

1. **문제 해결을 위해 고민한 내용**  
   - Nginx에서 **인증 절차**를 구현하기 위한 방법을 찾아야 함.  
   - 검색을 통해 Nginx에서 사용자 인증을 제공하는 **htpasswd** 방법을 발견함.  
   - 참고한 자료: [htpasswd로 ID, 패스워드 생성시에 SHA 암호화방식으로 암호생성하기](https://www.linux.co.kr/bbs/board.php?bo_table=lecture&wr_id=3124)

2. **htpasswd 설치 방법**  
   - **htpasswd**는 `apache2-utils` 패키지에 포함되어 있습니다.  
   - 다음 명령어를 사용해 설치합니다:  
     ```bash
     # Ubuntu/Debian
     sudo apt update
     sudo apt install apache2-utils
     ```
   - 설치 확인:  
     ```bash
     htpasswd -v
     ```

3. **Docker Compose에서 Nginx 인증 설정**  
   다음은 Nginx 컨테이너에 `htpasswd` 파일과 설정 파일을 마운트하는 `docker-compose.yml` 예시입니다:  

   ```yaml
   nginx:
     image: nginx:latest
     container_name: nginx_proxy
     ports:
       - "80:80"  # Nginx의 HTTP 요청
     volumes:
       - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro  # Nginx 설정 파일 마운트
       - ./nginx/.htpasswd/.htpasswd:/etc/nginx/.htpasswd:ro  # htpasswd 파일 마운트
       - ./wait-for-it.sh:/wait-for-it.sh  # wait-for-it.sh 스크립트 마운트
     depends_on:
       - mlflow
       - jupyter  # Jupyter 의존 추가
     networks:
       - app_network
     command: ["./wait-for-it.sh", "mlflow:5000", "--timeout=30", "--", "./wait-for-it.sh", "jupyter:8888", "--timeout=30", "--", "nginx", "-g", "daemon off;"]
   ```

4. **Nginx 설정 파일 (`nginx.conf`) 예시**  
   `nginx.conf`에서 `htpasswd`를 사용하여 사용자 인증을 설정합니다:  

   ```nginx
    # MLFlow 라우팅
    location /mlflow/ {
        rewrite ^/mlflow/(.*)$ /$1 break;
        proxy_pass http://mlflow/;  # 포트 5000으로 전달
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Basic Authentication 설정
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
   ```

5. **htpasswd 파일 생성**  
   Nginx가 사용할 인증 파일을 생성합니다:  
   ```bash
   sudo mkdir -p ./nginx/.htpasswd
   sudo htpasswd -c ./nginx/.htpasswd/.htpasswd 사용자이름
   ```

---

## TO-BE
최종 결과입니다.

1. **아웃풋 (결과)**  
   - MLflow 서버에 접속하려면 **설정된 아이디와 비밀번호**를 입력해야 함.  
   - 인증 절차가 추가되어 불필요한 접근을 차단하고 보안이 강화됨.  
   - `docker-compose`를 통해 Nginx 컨테이너가 의존하는 서비스가 모두 준비될 때까지 대기한 후 실행됨.  
   - EC2 인스턴스의 Nginx를 통해 인증이 적용된 안정적인 **MLflow** 서버를 제공함.

    <img src="https://github.com/online5880/tistory_post/blob/main/202412/project/htpasswd.png?raw=true" width="100%" height="100%"/>