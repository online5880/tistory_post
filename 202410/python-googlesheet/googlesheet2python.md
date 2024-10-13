
# 파이썬으로 Google 스프레드시트 연동하기

Google 스프레드시트를 파이썬으로 연동하면 데이터를 편리하게 관리할 수 있음. 

이 글에서는 Google API를 통해 파이썬으로 Google 스프레드시트에 접근하는 방법을 단계별로 설명함.

## 1. Google API 설정하기

먼저 Google Cloud Platform(GCP)에서 API를 사용하도록 설정해야 함. 아래의 절차를 따라 설정함.

1. [Google Cloud Console](https://console.cloud.google.com/)에 접속하여 새 프로젝트를 생성함.

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/01_gcp_home.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/02-01_create_project.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/02-02_create_project.png?raw=true" width="100%" height="100%"/>


2. **모든 API 보기** 로 이동한 뒤, `Google Sheets API`를 검색하여 활성화함.

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/03_all_api.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/04_search_sheet.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/05_activate_api.png?raw=true" width="100%" height="100%"/>

3. **API 및 서비스** > **사용자 인증 정보** > **서비스 계정 관리** 에서 **서비스 계정을** 생성함.

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/06_user_auth.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/07_manage_user.png?raw=true" width="100%" height="100%"/>

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/08-01_create_service_account.png?raw=true" width="100%" height="100%"/>
    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/08-02_create_service_account.png?raw=true" width="100%" height="100%"/>
    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/08-03_create_service_account.png?raw=true" width="100%" height="100%"/>
    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/08-04_create_service_account.png?raw=true" width="100%" height="100%"/>
    

4. **서비스 계정**을 생성한 후, `.json` 파일로 인증 키를 다운로드함.

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/09-01_manage_key.png?raw=true" width="100%" height="100%"/>
    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/09-02_manage_key.png?raw=true" width="100%" height="100%"/>
    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/09-03_manage_key.png?raw=true" width="100%" height="100%"/>

이 `.json` 파일이 나중에 파이썬 코드에서 필요함.

5. **IAM**에서 앞에서 생성 이메일 주소 복사

    <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/10_IAM.png?raw=true" width="100%" height="100%"/>

6. 불러올 **구글 스프레드 시트** 페이지 열고 **공유** > **복사한 이메일 추가**

     <img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/11_share_sheet.png?raw=true" width="100%" height="100%"/>

## 2. 파이썬 환경 설정

Google 스프레드시트 API와 상호작용하기 위해 몇 가지 패키지가 필요함. 터미널에서 아래 명령어로 필요한 패키지를 설치함.

```bash
pip install gspread oauth2client
pip gspread
```

## 3. 코드 작성

아래는 Google 스프레드시트에 접근하여 데이터를 읽고 쓰는 파이썬 코드 예시

```python
import pandas as pd
from oauth2client.service_account import ServiceAccountCredentials
import gspread

# 인증 설정
# Google 스프레드시트 API 인증 범위 설정
scope = ['https://spreadsheets.google.com/feeds']

# 서비스 계정 JSON 키 파일 경로 설정
json_key_path = "다운로드 받은 키.json"	

# 인증 정보 로드
credential = ServiceAccountCredentials.from_json_keyfile_name(json_key_path, scope)
gc = gspread.authorize(credential)

# 스프레드시트 URL 설정
spreadsheet_url = "스프레드시트 URL"

# URL을 통해 스프레드시트 열기
doc = gc.open_by_url(spreadsheet_url)

# 시트 선택하기
sheet = doc.worksheet("시트이름")

# 데이터 프레임으로 변환하기 (데이터프레임으로 불러오고 싶을 경우)
df = pd.DataFrame(sheet.get_all_values())

# 첫 번째 행을 컬럼명으로 설정하고 나머지 데이터로 변환
df.rename(columns=df.iloc[0], inplace=True)
df.drop(df.index[0], inplace=True)

# 데이터 프레임 출력
df.head()
```

위 코드에서 `"다운로드 받은 키.json"` 부분에 앞서 다운로드한 서비스 계정 키 파일 경로를 입력해야 함.

## `APIError: APIErorr: [400]:`  발생할 경우
- 구글 스프레드 시트가 xlsx 또는 다른 확장자로 되어 있기 때문이다.
- 해결방법 : 구글 스프레드 시트로 변경해준다.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/12_error400.png?raw=true" width="100%" height="100%"/>

<img src="https://github.com/online5880/tistory_post/blob/main/202410/python-googlesheet/images/13_save_sheet.png?raw=true" width="100%" height="100%"/>

## 4. 결론

이제 파이썬으로 Google 스프레드시트에 데이터를 자유롭게 읽고 쓸 수 있음. 이를 통해 데이터 분석이나 자동화 작업을 쉽게 처리할 수 있음. 더 자세한 내용은 [Google Sheets API 문서](https://developers.google.com/sheets/api)를 참고할 수 있음.

## 참고 자료

- [Google Sheets API 공식 문서](https://developers.google.com/sheets/api)
- [Google Cloud Console](https://console.cloud.google.com/)
- [OAuth2 인증 관련 문서](https://developers.google.com/identity/protocols/oauth2)
