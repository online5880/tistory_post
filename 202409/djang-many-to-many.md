
# Django 다대다(Many-to-Many) 관계 예시

## 1. 서론

이번 포스트 Django에서 다대다(Many-to-Many) 관계를 사용하는 모델의 예시입니다.

팀프로젝트를 하다가 데이터베이스를 설계?를 하려다보니 잘 모르는 부분이라 우선 정리를 해봅니다.

이 예시는 강의(Lecture)와 키워드(Keyword) 간의 다대다 관계를 정의한 것입니다. 한 강의는 여러 키워드를 가질 수 있고, 하나의 키워드는 여러 강의에 연결될 수 있습니다.

### Keyword 모델

```python
class Keyword(models.Model):
    keyword = models.CharField(max_length=255)  # 키워드
```

- `Keyword` 모델은 키워드를 나타냅니다. `keyword` 필드는 각 키워드를 저장하는 문자열 필드입니다.
- 이 모델은 강의(Lecture)와 다대다 관계를 가집니다.

### Lecture 모델

```python
class Lecture(models.Model):
    lecture_title = models.CharField(max_length=255)  # 강의 제목
    lecture_content = models.TextField()  # 강의 내용
    lecture_summary = models.TextField()  # 강의 요약
    thumbnail_url = models.URLField(max_length=255)  # 썸네일 이미지 URL
    semester = models.CharField(max_length=50)  # 강의 학기 (예: "2024-1학기")

    # Many-to-Many 관계 정의
    keywords = models.ManyToManyField(Keyword, related_name="lectures")
```

- `Lecture` 모델은 강의를 나타냅니다. 강의 제목, 강의 내용, 요약, 썸네일 이미지 URL, 그리고 강의 학기 정보를 저장합니다.
- `keywords` 필드는 `Keyword` 모델과 다대다 관계를 설정합니다. 
- 즉, 하나의 강의는 여러 키워드를 가질 수 있고, 여러 키워드가 하나의 강의에 연결될 수 있습니다. 
- `related_name="lectures"`를 통해 키워드에서 연결된 강의를 쉽게 참조할 수 있습니다.

## 2. 다대다 관계 설정

### Many-to-Many 관계란?

Django에서 `ManyToManyField`는 두 모델 간의 다대다 관계를 정의할 때 사용됩니다. 다대다 관계는 중간 테이블을 통해 관리되며, Django는 이를 자동으로 처리합니다.

위의 예시에서는 `Lecture`와 `Keyword` 간의 다대다 관계가 설정되었습니다. 이는 하나의 강의가 여러 키워드를 가질 수 있으며, 하나의 키워드가 여러 강의에 할당될 수 있음을 의미합니다.

### 관계 설정 예시

#### 강의에 키워드 추가하기

```python
# 새로운 강의 생성
lecture = Lecture.objects.create(
    lecture_title="Python 기초",
    lecture_content="Python 프로그래밍의 기초 내용을 다룹니다.",
    lecture_summary="Python 기초 강의 요약",
    thumbnail_url="http://example.com/thumbnail.jpg",
    semester="2024-1학기"
)

# 새로운 키워드 생성
keyword_python = Keyword.objects.create(keyword="Python")
keyword_programming = Keyword.objects.create(keyword="프로그래밍")

# 강의에 키워드 추가
lecture.keywords.add(keyword_python, keyword_programming)
```

이 예시는 `Python 기초`라는 제목의 새로운 강의를 생성하고, `Python` 및 `프로그래밍`이라는 두 개의 키워드를 강의에 추가하는 방법을 보여줍니다.

#### 특정 키워드에 연결된 강의 조회하기

```python
# 키워드 "Python"에 연결된 모든 강의 조회
keyword_python = Keyword.objects.get(keyword="Python")
lectures = keyword_python.lectures.all()

for lecture in lectures:
    print(lecture.lecture_title)
```

이 쿼리는 `Python` 키워드에 연결된 모든 강의를 조회하여 강의 제목을 출력합니다.

## 3. 결론

Django의 `ManyToManyField`는 다대다 관계를 쉽게 관리할 수 있게 도와줍니다. 

위의 강의와 키워드 예시처럼, 서로 여러 관계를 가질 수 있는 엔티티 간의 관계를 설정할 때 매우 유용합니다. Django는 이러한 관계를 자동으로 처리하며, 관련된 쿼리를 효율적으로 실행할 수 있도록 지원합니다.
