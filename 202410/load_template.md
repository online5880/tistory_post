
## LangChain - `load_prompt` 함수 예시

LangChain의 `load_prompt` 함수를 사용하여 다양한 파일 형식(`YAML`, `JSON`, `.txt`, `.py`)으로 프롬프트 템플릿을 불러오고, 이를 채팅 모델에서 사용하는 방법을 설명합니다. `load_prompt`는 지정된 파일 형식에 맞추어 템플릿을 동적으로 로드할 수 있는 기능을 제공함.

---

### 1. YAML 파일 예시
YAML 파일 형식으로 템플릿을 정의하고, `load_prompt`로 불러올 수 있음.

- **파일명**: `template.yaml`

```yaml
template: |
  {name}님, 오늘의 날씨는 {weather}입니다. 
  좋은 하루 보내세요!
variables:
  - name
  - weather
```

**Python 코드에서 사용하기:**

```python
from langchain_core.prompts import load_prompt
from langchain_community.chat_models import ChatOpenAI

# YAML 파일에서 템플릿 불러오기
prompt = load_prompt("template.yaml")
model = ChatOpenAI()

# 템플릿에 필요한 변수 전달
response = prompt.invoke({"name": "지수", "weather": "맑음"})
print(response)
```

---

### 2. JSON 파일 예시
JSON 파일 형식으로 템플릿을 설정하고 `load_prompt`로 불러와 사용할 수 있음.

- **파일명**: `template.json`

```json
{
  "template": "{name}님, 오늘의 추천 음식은 {food}입니다. 맛있게 드세요!",
  "variables": ["name", "food"]
}
```

**Python 코드에서 사용하기:**

```python
from langchain_core.prompts import load_prompt
from langchain_community.chat_models import ChatOpenAI

# JSON 파일에서 템플릿 불러오기
prompt = load_prompt("template.json")
model = ChatOpenAI()

# 템플릿에 필요한 변수 전달
response = prompt.invoke({"name": "지수", "food": "비빔밥"})
print(response)
```

---

### 3. 텍스트 파일 (.txt) 예시
간단한 텍스트 파일을 `load_prompt`를 통해 불러와 그대로 활용할 수 있음.

- **파일명**: `template.txt`

```text
오늘의 추천 영화는 "인셉션"입니다. 즐거운 감상 되세요!
```

**Python 코드에서 사용하기:**

```python
from langchain_core.prompts import load_prompt
from langchain_community.chat_models import ChatOpenAI

# 텍스트 파일에서 템플릿 불러오기
prompt = load_prompt("template.txt")
model = ChatOpenAI()

# 템플릿 실행
response = prompt.invoke()
print(response)
```

---

### 4. Python 코드 파일 (.py) 예시
`PromptTemplate`을 사용하여 Python 코드에서 템플릿을 정의하고 사용할 수 있음.

- **파일명**: `template.py`

```python
from langchain.prompts import PromptTemplate

# 템플릿 작성
template = PromptTemplate(
    template="{name}님, 오늘은 {weather} 날씨입니다. 산책을 추천드려요!",
    input_variables=["name", "weather"]
)
```

**Python 코드에서 사용하기:**

```python
from template import template  # 작성한 템플릿을 import
from langchain_community.chat_models import ChatOpenAI

model = ChatOpenAI()

# 템플릿에 필요한 변수 전달
response = template.invoke({"name": "지수", "weather": "화창한"})
print(response)
```

---

위 예시들은 LangChain의 `load_prompt` 함수를 통해 다양한 파일 형식에서 템플릿을 불러와 활용하는 방법을 보여줌. 이 기능을 이용하여 동적인 템플릿 로딩 및 관리를 효율적으로 할 수 있음.
