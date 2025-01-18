# 파이썬으로 단일 연결 리스트(Linked List) 구현하기

연결 리스트(Linked List)는 데이터 구조의 기본 중 하나로, 각 요소(Node)가 데이터와 다음 노드의 주소(참조)를 포함하는 방식으로 연결된 자료 구조임. 

파이썬으로 단일 연결 리스트를 구현하는 방법을 예제 코드와 함께 설명함

---

## 코드: LinkedList 클래스 구현

아래는 `Node` 클래스와 `LinkedList` 클래스를 정의하여 연결 리스트의 기본 동작(추가, 조회, 삭제 등)을 구현한 코드임

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
    

class LinkedList:
    def __init__(self, value):
        self.head = Node(value)
        
    # LinkedList 의 가장 끝에 있는 노드에 새로운 노드 연결
    def append(self, value):
        cur = self.head
        while cur.next is not None:
            cur = cur.next
            
        cur.next = Node(value)
    
    # 모든 노드 출력
    def print_all(self):
        cur = self.head
        while cur is not None:
            print(cur.data)
            cur = cur.next
            
    # 노드 인덱스 출력
    def get_node(self, index):
        cur = self.head
        cur_idx = 0
        while cur_idx != index:
            cur = cur.next
            cur_idx += 1
        return cur
    
    # 노드 추가
    def add_node(self, index, value):
        new_node = Node(value)
        
        if index == 0:
            new_node.next = self.head
            self.head = new_node
            return
        
        prev_node = self.get_node(index-1)
        
        next_node = prev_node.next
        
        prev_node.next = new_node
        new_node.next = next_node
    
    # 노드 삭제
    def delete_node(self, index):
        if index == 0:
            self.head = self.head.next
            return
        prev_node = self.get_node(index-1)
        prev_node.next = prev_node.next.next
```

---

## 주요 기능 설명

1. **초기화**
   - `LinkedList`는 생성 시 첫 번째 노드(`head`)를 초기화함

2. **노드 추가 (`append`)**
   - 연결 리스트의 끝에 새 노드를 추가함

3. **모든 노드 출력 (`print_all`)**
   - 연결 리스트의 모든 노드를 순서대로 출력함

4. **특정 인덱스 노드 가져오기 (`get_node`)**
   - 주어진 인덱스의 노드를 반환함

5. **특정 인덱스에 새 노드 추가 (`add_node`)**
   - 주어진 인덱스 위치에 새 노드를 삽입함

6. **특정 인덱스 노드 삭제 (`delete_node`)**
   - 주어진 인덱스의 노드를 삭제함

---

## 실행 예제

아래는 위 코드로 연결 리스트를 생성하고 조작하는 과정의 실행 결과임

```python
# 초기 연결 리스트 생성
linked_list = LinkedList(5)  # [5]

# 노드 추가
linked_list.append(12)  # [5] -> [12]
linked_list.append(8)   # [5] -> [12] -> [8]

# 모든 노드 출력
linked_list.print_all()
# 출력: 5 12 8

print()

# 인덱스 1 위치에 새 노드 추가
linked_list.add_node(1, 6)  # [5] -> [6] -> [12] -> [8]
linked_list.print_all()
# 출력: 5 6 12 8

print()

# 인덱스 0 위치에 새 노드 추가
linked_list.add_node(0, 99)  # [99] -> [5] -> [6] -> [12] -> [8]
linked_list.print_all()
# 출력: 99 5 6 12 8

print()

# 인덱스 1 위치의 노드 삭제
linked_list.delete_node(1)  # [99] -> [6] -> [12] -> [8]
linked_list.print_all()
# 출력: 99 6 12 8

print()

# 인덱스 0 위치의 노드 삭제
linked_list.delete_node(0)  # [6] -> [12] -> [8]
linked_list.print_all()
# 출력: 6 12 8
```

---

## 실행 결과 요약

연결 리스트의 주요 작업을 순서대로 실행한 결과는 아래와 같음

1. **초기 리스트 생성**: `[5]`
2. **`append(12)`**: `[5] -> [12]`
3. **`append(8)`**: `[5] -> [12] -> [8]`
4. **`add_node(1, 6)`**: `[5] -> [6] -> [12] -> [8]`
5. **`add_node(0, 99)`**: `[99] -> [5] -> [6] -> [12] -> [8]`
6. **`delete_node(1)`**: `[99] -> [6] -> [12] -> [8]`
7. **`delete_node(0)`**: `[6] -> [12] -> [8]`

---

## 정리

연결 리스트는 데이터의 삽입과 삭제가 빈번한 작업에 유용한 자료 구조임. 위 코드를 활용하면 단일 연결 리스트의 기본 동작을 학습하고 다양한 문제를 해결하는 데 활용할 수 있음
