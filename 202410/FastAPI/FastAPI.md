
# FastAPI 사용하기

FastAPI는 Python으로 간단하게 API 서버를 구축할 수 있도록 도와주는 프레임워크로, Pydantic을 활용해 데이터 유효성 검사를 수행함. 

이번 글에서는 `FastAPI`와 `Pydantic`을 이용한 간단한 CRUD API 서버를 구현하고, 각 기능과 Swagger를 이용한 문서화에 대해 설명함.

## 코드 예제

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# 데이터 저장소 역할을 하는 딕셔너리
items = {
    0: {"name": "bread", "price": 1000},
    1: {"name": "water", "price": 500},
    2: {"name": "coffee", "price": 1200}
}

# Path Parameter
@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id not in items:
        return {"error": "아이템 정보가 없습니다."}
    return items[item_id]

# Path Parameter로 key 값 추가 조회
@app.get("/items/{item_id}/{key}")
def read_item_and_key(item_id: int, key: str):
    item = items.get(item_id)
    return item.get(key, "키에 해당하는 값이 없습니다.")

# Query Parameter
@app.get("/item-by-name")
def read_item_by_name(name: str):
    for item in items.values():
        if item['name'] == name:
            return item
    return {"error": "data not found"}

# 아이템 생성 모델 정의
class Item(BaseModel):
    name: str
    price: int

# 아이템 생성 (Create)
@app.post("/items/{item_id}")
def create_item(item_id: int, item: Item):
    if item_id in items:
        return {"error": "이미 등록된 아이템입니다."}
    items[item_id] = item.dict()
    return {"success": "등록 성공"}

# 업데이트 모델 정의 (일부 업데이트 가능하도록 Optional 사용)
class ItemForUpdate(BaseModel):
    name: Optional[str] = None
    price: Optional[int] = None

# 아이템 업데이트 (Update)
@app.put("/items/{item_id}")
def update_item(item_id: int, item: ItemForUpdate):
    if item_id not in items:
        return {"error": f"아이템 정보가 없습니다. : {item_id}"}
    if item.name:
        items[item_id]['name'] = item.name
    if item.price:
        items[item_id]['price'] = item.price
    return {"success": "아이템 업데이트 성공"}

# 아이템 삭제 (Delete)
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    if item_id in items:
        del items[item_id]
        return {"success": "아이템 삭제 성공"}
    return {"error": "아이템이 없습니다."}
```

## 주요 기능 설명

### 1. **CRUD 메서드**
   - `POST` : `/items/{item_id}` - 새로운 아이템을 생성함.
   - `GET` : `/items/{item_id}`, `/items/{item_id}/{key}`, `/item-by-name` - 아이템 조회 기능.
   - `PUT` : `/items/{item_id}` - 아이템 업데이트.
   - `DELETE` : `/items/{item_id}` - 아이템 삭제.

### 2. **Path Parameter와 Query Parameter**

   - **Path Parameter 예시**: `http://127.0.0.1:8000/items/0`와 같은 형식으로 `item_id`를 직접 경로에 포함시켜 특정 아이템을 조회함. 예를 들어 `http://127.0.0.1:8000/items/1/name`와 같은 URL을 통해 아이템 `id`와 속성 `key`를 Path Parameter로 조회 가능.
   - **Query Parameter 예시**: `http://127.0.0.1:8000/item-by-name?name=bread`와 같이, 경로 뒤에 `?`를 붙이고 `key=value` 형식으로 데이터를 전달하여 요청함. `name`이라는 쿼리 파라미터로 이름이 일치하는 아이템을 검색함.

### 3. **Pydantic 모델 사용**
   - `BaseModel`을 상속받아 데이터 유효성 검사를 간단하게 적용 가능. `Item` 모델을 사용해 `name`과 `price` 필드를 필수로 받고, `ItemForUpdate` 모델을 통해 업데이트 시 일부 필드만 선택적으로 받도록 함.

## Swagger UI로 API 문서 확인하기

FastAPI는 `/docs` 경로에서 자동으로 API 문서를 생성해주는 Swagger UI를 지원함. Swagger UI는 API 사용 방법과 요청/응답 형식을 인터페이스로 제공하여 API 문서화와 테스트를 쉽게 할 수 있도록 돕는 도구임.

Swagger UI를 통해 API 서버에 정의된 모든 엔드포인트와 요청 매개변수, 응답 데이터를 확인할 수 있음. 이를 통해 사용자와 개발자가 API의 구조와 데이터를 쉽게 이해하고, 기능 테스트도 가능함.

### Swagger UI 확인 방법
1. FastAPI 서버를 실행한 뒤, 브라우저에서 `http://127.0.0.1:8000/docs`로 접속.
2. 각 엔드포인트에 대한 설명과 사용 방법이 자동으로 생성되어 있으며, 각 API를 직접 호출하여 테스트할 수 있음.

### Swagger 예시 화면
Swagger는 아래와 같은 화면에서 각 API의 요청 방식, 파라미터 및 응답 예시를 확인할 수 있음.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/FastAPI/Swagger.png?raw=true" width="100%" height="100%"/>

## 실행 방법

1. 필요한 패키지 설치:
   ```bash
   pip install fastapi pydantic uvicorn
   ```

2. 서버 실행:
   ```bash
   uvicorn main:app --reload
   ```
   위 명령어에서 `main`은 Python 파일의 이름을 의미하며, `app`은 FastAPI 인스턴스의 이름임. `--reload` 옵션을 사용하면 코드 변경 시 자동으로 서버가 재시작됨.

3. Swagger UI 접속:
   서버가 실행되면, 브라우저에서 [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)로 접속하여 Swagger UI로 API를 테스트할 수 있음.

## 마무리

이 글에서는 FastAPI와 Pydantic을 활용해 CRUD 기능을 구현하고, Swagger UI를 이용해 API 문서를 자동 생성하는 방법에 대해 설명함. 이 예제를 통해 FastAPI의 간단하고 효율적인 API 개발과 문서화 기능을 이해하는 데 도움이 되길 바람.
