# 1. 파이썬 비동기 프로그래밍

## 1.1. 기본 개념 및 이벤트 루프
비동기 프로그래밍은 I/O 작업 등 시간이 소요되는 작업을 다른 작업과 병렬로 처리하여 전체 프로그램의 응답성을 높이는 방식입니다. 

파이썬에서는 `asyncio` 모듈을 사용하여 비동기 작업을 구현합니다.

- coroutine: `async def`로 정의하며, 내부에서 `await`를 사용하여 다른 코루틴의 완료를 기다립니다.
- event loop: 비동기 작업을 스케줄링하고 실행하는 중심 메커니즘입니다.

## 1.2. 간단한 비동기 함수 예제

```python
import asyncio # asyncio 임포트

async def say(message, delay):
    print(f"'{message}' 출력 대기 중 ({delay}초)")
    await asyncio.sleep(delay)  # 비동기적으로 delay 만큼 대기
    print(message)

async def main():
    print("비동기 함수 실행 시작")
    await say("첫번째 메시지", 1)
    await say("두번째 메시지", 2)
    print("비동기 함수 실행 완료")

asyncio.run(main())
```

출력 결과

```shell
비동기 함수 실행 시작
'첫번째 메시지' 출력 대기 중 (1초)
첫번째 메시지
'두번째 메시지' 출력 대기 중 (2초)
두번째 메시지
비동기 함수 실행 완료
```

해설
- say 함수는 주어진 delay 동안 대기한 후 메시지를 출력합니다.
- main 함수에서는 await를 사용해 코루틴이 순차적으로 실행되지만, 이를 활용해 다른 작업과 결합할 수도 있습니다.

---

# 2. 동시실행 : 여러 작업을 동시에 처리
## 2.1 `async.gather` 사용하기

```python
import asyncio

async def task(task_id, delay):
    print(f"Task {task_id} 시작 (대기 {delay}초)")
    await asyncio.sleep(delay)
    print(f"Task {task_id} 완료")
    return f"결과 {task_id}"

async def main():
    tasks = [task(i, i) for i in range(1, 4)]
    results = await asyncio.gather(*tasks)
    print("모든 작업 완료:", results)

asyncio.run(main())
```

출력 결과
  
```shell
# 동시에 태스크 실행
Task 1 시작 (대기 1초)
Task 2 시작 (대기 2초)
Task 3 시작 (대기 3초)

# 대기 시간에 따라서 순차적으로 출력
Task 1 완료
Task 2 완료
Task 3 완료

모든 작업 완료: ['결과 1', '결과 2', '결과 3']
```

해설
- 여러 작업을 리스트에 담은 후 `asyncio.gather`로 동시에 실행하면, 각 작업의 지연시간 동안 다른 작업이 실행됩니다.

---

# 3. async 로 이메일 보내기

```python
import asyncio
from email.message import EmailMessage
import aiosmtplib
import os
from dotenv import load_dotenv

load_dotenv()  # .env 파일 로드

username = os.getenv("EMAIL_USER")
password = os.getenv("EMAIL_PASSWORD")

async def send_email(subject: str, msg: str):
    # 이메일 메시지 구성
    message = EmailMessage()
    message["From"] = os.getenv("EMAIL_USER")  # 발신자 이메일
    message["To"] = "gjtjqkr5880@gmail.com"
    message["Subject"] = f"{subject}"
    message.set_content(f"{msg}")

    # SMTP 서버 정보
    smtp_host = "smtp.naver.com"
    smtp_port = 465
    
    await aiosmtplib.send(
        message,
        hostname="smtp.naver.com",
        port=465,
        use_tls=True,  # SSL/TLS를 초기부터 사용
        username=username,
        password=password,
    )
    print("이메일 전송 완료")

asyncio.run(send_email("제목","내용"))
```