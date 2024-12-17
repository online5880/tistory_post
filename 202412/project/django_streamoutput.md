## **Django에서 OpenAI API로 LLM 스트림 출력 시 실시간 출력 문제 해결**

---

### **AS-IS**
- **문제상황 인지**  
  Django를 사용하여 OpenAI API로 LLM의 스트림 출력 구현 시, HTML에서 실시간 출력이 되지 않는 문제 발생  
- **해결하려고 하는 문제**  
  스트림 데이터를 HTML에서 지연 없이 실시간으로 출력  
- **만들고 싶은 기능**  
  OpenAI API의 스트림 결과를 HTML 화면에 실시간으로 출력  

---

### **Challenge**
- **문제해결을 위해 고민한 내용**  
  - Django에서 OpenAI API를 사용해 스트림 데이터를 반환  
  - HTML에서 서버로부터 전달된 스트림 데이터를 실시간으로 렌더링  
  - Nginx를 사용한 리버스 프록시 설정 문제 확인  

- **어떻게 기술적으로 해결했는지**  
  Nginx 설정에서 `proxy_buffering` 옵션이 켜져 있어서 스트림 데이터가 실시간으로 출력되지 않았음.  
  해당 옵션을 끄기 위해 Nginx 설정 파일에 다음과 같이 수정함:  
  ```nginx
  location / {
      proxy_pass http://web:8000;
      proxy_buffering off;  # 스트림 데이터를 실시간으로 전달
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

---

### **TO-BE**
- **아웃풋 (결과)**
- HTML 화면에서 OpenAI API로 받은 스트림 데이터가 실시간으로 출력됨.
- `proxy_buffering off;` 를 통해 Nginx가 데이터를 버퍼링하지 않고 즉시 전달하도록 설정함으로써 문제 해결.