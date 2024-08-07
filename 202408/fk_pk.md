# SQL 기본 키와 외래 키
## 기본 키(Primary Key, PK)
기본키는 테이블 내의 각 행을 고유하게 식별하는 데 사용되는 열 또는 열 집합이다.

기본키는 고유해야 하며, NULL 값을 가질 수 없다.

### 기본 키의 특징
- **고유성** : 각 행을 고유하게 식별한다. 즉, 중복값 존재 X
- **NULL 불가** : 기본 키는 NULL 값을 가질 수 없다.

### 기본 키 예제
```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100),
    BirthDate DATE
);
```
위 예제에서는 `StudentID` 열은 `Student` 테이블의 기본 키로 설정되어 있다.

따라서 `StudentID` 는 각 학생을 고유하게 식별하며, NULL 값을 가질 수 없다.

<img src="https://github.com/online5880/tistory_post/blob/main/Images/TablePlus/Students.png?raw=true" width="100%" height="100%"/> 

---
## 외래 키(Foreign Key, FK)
외래 키는 한 테이블의 열이 다른 테이블의 기본 키를 참조하도록 설정된 열이다.

두 테이블 간의 관계를 정의하는 데 사용된다.

### 외래 키의 특징
- **무결성** : 외래 키는 참조하는 기본 키가 존재해야 한다.
- **다른 테이블과의 관계** : 외래 키는 두 테이블 간의 관계를 정의한다.
- **NULL 가능성** : 외래키는 NULL 값을 가질 수 있다.

### 외래 키 예제
```sql
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);
```
위의 예제에서 StudentID 열은 Enrollments 테이블에서 외래키로 설정되어 있으며, 이는 Students 테이블의 StudentID 열을 참조한다.

이렇게 설정함으로써 Enrollments 테이블의 StudentID 값은 Students 테이블에 존재해야 한다.

<img src="https://github.com/online5880/tistory_post/blob/main/Images/TablePlus/Enrollments.png?raw=true" width="100%" height="100%"/> 

---
## 기본 키와 외래 키의 관계
### 예제 코드
```sql
CREATE TABLE Students(
  StudentID INT PRIMARY KEY,
  Name VARCHAR(100),
  BirthDate DATE
);


create table Enrollments (
  EnrollmentID INT PRIMARY KEY,
  StudentID INT,
  CourseID INT,
  FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
  FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);


create table Courses(
  CourseID INT PRIMARY KEY,
  CourseName VARCHAR(100)
);
```
이 예제에서는 Students 테이블과 Courses 테이블 간의 관계를 Enrollments 테이블을 통해 관리한다.

이를 통해 데이터베이스는 학생과 코스 간의 등록 관계를 효율적으로 관리할 수 있다.

<img src="https://github.com/online5880/tistory_post/blob/main/Images/TablePlus/Enrollments_Diagram.png?raw=true" width="100%" height="100%"/> 

---

## 결론
기본키와 외래키는 데이터베이스 설계의 핵심 요소이다.

기본키는 테이블의 각 행을 고유하게 식별하고, 외래키는 테이블 간의 관계를 정의한다.

이를 통해 데이터의 무결성을 유지하고, 효율적인 데이터베이스 관리를 할 수 있다.

---
## 참고사이트 및 영상
[[MSSQL 중급 1강] 기본키, 외래키](https://www.youtube.com/watch?v=DFS7FMcPsy8)]

[[SQL] 테이블 제약조건 & 기본키, 외래키 지정방법 (PRIMARY KEY, FOREIGN KEY)](https://velog.io/@estell/SQL-%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%A0%9C%EC%95%BD%EC%A1%B0%EA%B1%B4)

[SQL FOREIGN KEY(외래 키)](https://m.blog.naver.com/kijun/222023302407)

<!-- <img src="이미지 주소" width="50%" height="50%"/> -->