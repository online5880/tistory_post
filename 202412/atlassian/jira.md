
# Google Sheets WBS 데이터를 Jira Board에 연동하기

## 개요
Google Sheets에 작성된 WBS(Work Breakdown Structure) 데이터를 Atlassian의 Jira API를 활용하여 자동으로 Jira Board에 태스크로 등록하는 Python 스크립트를 작성하는 방법을 소개함. 이 작업은 반복적인 데이터 입력 작업을 줄이고, 프로젝트 관리의 효율성을 높이는 데 기여함.

---

## 사전 준비
### 1. Jira API 토큰 생성
Jira 계정에서 **API 토큰**을 생성해야 함. 
[Atlassian API 토큰 생성 가이드](https://id.atlassian.com/manage-profile/security/api-tokens)를 참고하여 토큰을 발급받을 수 있음.

---

## 주요 코드

Google Sheets에서 가져온 WBS 데이터를 사용하여 Jira API를 통해 태스크를 생성하는 코드임.

```python
from datetime import datetime
import requests
import json
import pandas as pd

# Jira API 설정
JIRA_HOST = 'https://your-jira-instance.atlassian.net'
API_URL = f"{JIRA_HOST}/rest/api/3/issue"
EMAIL = "your-email@example.com"  # Jira 로그인 이메일
API_TOKEN = "your-api-token"  # 생성한 API 토큰
PROJECT_KEY = "YOUR_PROJECT_KEY"  # Jira 프로젝트 키

def validate_date_format(date_str):
    # 날짜 형식 검증 및 변환
    try:
        return datetime.strptime(date_str, "%Y-%m-%d").strftime("%Y-%m-%d")
    except ValueError:
        raise ValueError(f"날짜 형식이 잘못됨: {date_str}. 'YYYY-MM-DD' 형식이어야 함.")

def create_jira_task(row):
    # Jira 태스크 생성
    start_date = validate_date_format(row["Start Date"])
    
    description_content = {
        "type": "doc",
        "version": 1,
        "content": [
            {
                "type": "paragraph",
                "content": [
                    {
                        "type": "text",
                        "text": f"태스크 번호: {row['Task Number']}
담당자: {row['Assignee']}"
                    }
                ]
            }
        ]
    }

    issue_data = {
        "fields": {
            "project": {"key": PROJECT_KEY},
            "summary": f"[{row['Task Number']}] {row['Task Name']}",
            "description": description_content,
            "issuetype": {"name": row["Type"]},
            "customfield_10015": start_date,
            "customfield_10001": row["Team"],
        }
    }

    response = requests.post(
        API_URL,
        headers={"Content-Type": "application/json"},
        auth=(EMAIL, API_TOKEN),
        data=json.dumps(issue_data)
    )
    
    if response.status_code == 201:
        print(f"이슈 생성 성공: {response.json().get('key')} - {row['Task Name']}")
    else:
        print(f"이슈 생성 실패: {row['Task Name']}")
        print("응답 코드:", response.status_code)
        print("응답 내용:", response.text)

def main():
    # CSV 데이터 로드
    tasks = pd.read_csv('task_list.csv')  # 'task_list.csv' 경로 확인 필요
    for _, row in tasks.iterrows():
        try:
            create_jira_task(row)
        except Exception as e:
            print(f"오류 발생: {e}")

if __name__ == "__main__":
    main()
```

---

## 실행 방법
1. 위 코드의 설정 부분에서 Jira 관련 정보를 업데이트해야 함.
2. `task_list.csv` 파일에 Google Sheets에서 저장한 WBS 데이터를 준비함.
3. Python 환경에서 필요한 라이브러리를 설치함.
   ```bash
   pip install requests pandas
   ```
4. 스크립트를 실행하여 Jira에 태스크가 생성되는지 확인함.

---

## 결과
Google Sheets에서 다운로드한 WBS 데이터가 **Jira Board**에 태스크로 등록됨. 태스크가 정상적으로 생성되었는지 확인하려면 Jira에서 태스크 리스트를 검토하면 됨.

---

## 결론
이 스크립트는 WBS 데이터를 Jira Board로 연동하여 프로젝트 관리의 효율성을 높이는 데 유용함. 수작업으로 입력하는 데 소요되는 시간을 줄이고, 데이터 일관성을 보장할 수 있음.

---

## 추천 해시태그
#Python #JiraAPI #프로젝트관리 #자동화 #WBS #API통합 #생산성향상 #Jira연동 #데이터관리 #파이썬활용
