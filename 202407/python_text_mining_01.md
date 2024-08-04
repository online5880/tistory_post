# 텍스트 마이닝
## 대통령 연설문 텍스트 마이닝
 Doit! 쉡게 배우는 파이썬 데이터 분석 ([도서](https://m.yes24.com/Goods/Detail/108947478))

 ## 텍스트 마이닝이란?
문자로 된 데이터에서 가치 있는 정보를 얻어 내는 분석 기법이다.

## 형태소 분리
- 텍스트 마이닝을 할 때 가장 먼저 하는 작업이다.
- 문장을 구성하는 어저들이 어떤 품사인지 파악하는 것.
  - 품사 : 명서, 대명사, 수사, 관형사, 부사, 감탄사, 조사, 서술격조사, 동사, 형용사 [[참고]](https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=barnbyul&logNo=221873072735)
  - 어절 : 띄어쓰기대로 여러 글자씩 [[참고]](https://m.blog.naver.com/pso164/222545822231)
  
## 프로젝트 세팅하기
### 01. `KoNLPy` 패키지 설치하기
- `자바` 가 설치되어 있어야 사용할 수 있다.
- [[MAC 한국어 자연어처리 KoNLPy 설치]](https://z2soo.tistory.com/174)
- [M1칩 Mac에서 KoNLPy 한국어 처리 파이썬 패키지 설치하기](https://velog.io/@yoonsy/M1%EC%B9%A9-Mac%EC%97%90-konlpy-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

### 02. 의존성 패키지란?
- 어떤 패키지는 다른 패키지의 기능을 이용하기 때문에 다른 패키지를 먼저 설치해야 작동한다.
- 이처럼 패키지가 의존하고 있는 패키지를 `의존성 패키지`라고 한다.
 ```python
 pip install jpype1
 pip install konlpy
 ```

## 텍스트 마이닝 시작
### 01. 연설문 불러오기
로컬에 저장되어 있는 `speech_moon.txt` 파일을 불러온다.
```python
moon = open('../data/speech_moon.text', encoding = 'utf-8').read()
```
> 해당 파일은 책에서 제공해주는 파일입니다.
### 02. 불필요한 문자 제거
- 특수문자, 한자, 공백 등은 분석 대상이 아니기 때문에 제거한다.
- 문자 처리 패키지인 `re`의 `sub()`를 이용해 한글이 아닌 문자를 공백으로 교체한다.
- [[Python] 정규표현식 (Regular Expression ; Regex)](https://cheris8.github.io/python/PY-Regex/)

```python
import re  # 문자 처리 패키지

moon = re.sub("[^가-힣]", " ", moon)

# re.sub() : 함수는 정규 표현식을 사용하여 문자열에서 패턴이 일치하는 부분을 다른 문자열로 대체한다.
# `'[^가-힣]'`는 정규 표현식 패턴
# [^가-힣] 은 한글이 아닌 모든 문자를 의미한다.
# ^ 는 부정을 의미하고 가-힣은 한글의 범위를 나타낸다.
# 정규 표현식이란?
# 특정한 규칙을 가진 문자열을 표현하는 언어이다.
```

### 03. 명사 추출하기
- `konlpy.tag.Hannaum()`의 `nouns()`를 이용해서 명사를 추출한다.
- 예시 코드

```python
# hannanum 만들기
import konlpy

hannanum = konlpy.tag.Hannanum()

hannanum.nouns("대한민국의 영토는 한반도와 그 부석도서로 한다")
```
```python
['대한민국', '영토', '한반도', '부석도서']
```

- 사용 코드
```python
nouns = hannanum.nouns(moon)
```
```python
['정권교체',
 '정치교체',
 '시대교체',
 '불비불명',
 '고사',
 '남쪽',
 '언덕',
 '나뭇가지',
 .....(생략)]
```

### 04. 데이터 프레임으로 변경하기
- 명사를 추출하고 출력 결과는 리스트 자료형이기 때문에 다루기 쉽도록 데이터 프레임으로 변환한다.
```python
import pandas as pd
df_word = pd.DataFrame({'word' : 'nouns'})
df_word
```
```python
	word
0	정권교체
1	정치교체
2	시대교체
...	...
1411	우리나라
1412	대통령
```

### 05. 단어 빈도표 만들기
- 어떤 단어를 많이 사용 했는지 확인한다.
- 한 글자로 된 단어는 의미가 없는 경우가 많기 때문에 제거한다.
- 글자 수(count) column 을 추가한다.

```python
# 글자 수 컬럼 추가
df_word['count'] = df_word['word'].str.len()
df_word
```

```python
	
word	count
712	국민	2
1164	가사	2
1163	숙제	2
...	...	...
707	평생학습체제	6
1268	군사대결지대	6
1055	국가일자리위원회	8
```

- 글자 수를 구했으니 이제 두 글자 이상인 단어만 남긴다.

```python
# 두 글자 이상 단어만 남기기
# df_word.query('count >= 2') , df_word = df_word[df_word['count] >= 2]
df_word = df_word.loc[df_word['count'] >= 2]
df_word.sort_values('count')

```
### 06. 단어 빈도 구하기

- 단어 사용 빈도를 구하고 빈도순으로 정렬한다.

```python
# 단어별 분리
# 빈도 구하기
# 내림차순 정렬

df_word = df_word.groudby('word', as_index = False).agg(n = ('word','count')).sort_values('n',ascending = False)

```

```python

word	n
153	나라	19
462	일자리	19
198	대통령	12
...	...	...
278	북핵문제	1
279	분단	1
355	수천	1
```

### 07. 단어 빈도 막대 그래프 그리기

- 상위 20개를 추출한다.

```python
top20 = df_word.head(20)
```

- `seaborn` 을 이용해서 막대 그래프를 만든다.
- 한글 폰트를 적용한다.

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 한글 폰트
# 해상도
# 가로 세로 크기

plt.rcParams.update(
    {"font.family": "AppleGothic", "figure.dpi": "120", "figure.figsize": [6.5, 6]}
)

sns.barplot(data=top20, x="n", y="word", palette="hls")

plt.show()
```

![alt text](https://github.com/online5880/tistory_post/blob/main/Images/text_mining_01.png?raw=true)

### 08. 워드 클라우드 만들기

- 워드 클라우드에서 단어는 키, 빈도는 값으로 구성된 딕셔너리 자료 구조를 이용해 만든다.
- `df_word` 를 딕셔너리로 변환해야한다.

```python
dic_word = df_word.set_index("word").to_dict()["n"]
dic_word
```

```python
{'나라': 19,
 '일자리': 19,
 '국민': 18,
 '우리': 17,
 ...
 '분야': 1,
 '분쟁': 1,
 '수천': 1}
```

```python
from wordcloud import WordCloud

font = "D2Coding-Ver1.3.2-20180524-all.ttc"

wc = WordCloud(
    font_path=font,  # 폰트
    width=400,  # 가로 크기
    height=400,  # 세로 크기
    background_color="white",  # 배경색
)

img_wordcloud = wc.generate_from_frequencies(dic_word)

plt.figure(figsize=(5, 5))  # 가로, 세로 크기 설정
plt.axis("off")  # 테두리 선 없애기
plt.imshow(img_wordcloud)  # 워드 클라우드 출력
```

![alt text](https://github.com/online5880/tistory_post/blob/main/Images/text_mining_02.png?raw=true)