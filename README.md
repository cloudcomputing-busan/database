# MySQL로 Database 실습하기

```
school.students;
+---------------+--------------+------+-----+---------+-------+
| Field         | Type         | Null | Key | Default | Extra |
+---------------+--------------+------+-----+---------+-------+
| student_id    | int          | NO   | PRI | NULL    |       |
| first_name    | varchar(50)  | NO   |     | NULL    |       |
| last_name     | varchar(50)  | NO   |     | NULL    |       |
| date_of_birth | date         | YES  |     | NULL    |       |
+---------------+--------------+------+-----+---------+-------+
classes.courses;
+---------------+---------------------------+------+-----+---------+-------+
| Field         | Type                      | Null | Key | Default | Extra |
+---------------+---------------------------+------+-----+---------+-------+
| course_id     | int                       | NO   | PRI | NULL    |       |
| course_name   | varchar(100)              | NO   |     | NULL    |       |
| professor_name| varchar(100)              | NO   |     | NULL    |       |
+---------------+---------------------------+------+-----+---------+-------+
enroll.enrollments;
+-----------------+-------------+------+-----+---------+-------+
| Field           | Type        | Null | Key | Default | Extra |
+-----------------+-------------+------+-----+---------+-------+
| enrollment_id   | int         | NO   | PRI | NULL    |       |
| student_id      | int         | YES  | MUL | NULL    |       |
| course_id       | int         | YES  | MUL | NULL    |       |
| enrollment_date | date        | YES  |     | NULL    |       |
+-----------------+-------------+------+-----+---------+-------+
```

## Database 접속하기
```
yum install maria105-server -y

ENDPOINT=<RDS Endpoint>
USER=<RDS 사용자 이름>

mysql -h $ENDPOINT -u $USER -p
```

## Database DB생성하기
```
create database school
create database classes
create database enroll
```

## 위와 같이 테이블 생성하기
```
use school;
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE,
    UNIQUE (first_name, last_name)
);

use classes;
CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    professor_name VARCHAR(100) NOT NULL,
    UNIQUE (course_name)
);

use enroll;
CREATE TABLE enrollments (
    enrollment_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id),
    UNIQUE (student_id, course_id)
);
```

## 테이블 조회
```
DESC school.students
DESC classes.courses
DESC enroll.enrollments
```

# STUDENTS TABLE
```
+------------+------------+-----------+----------------+
| student_id | first_name | last_name | date_of_birth  |
+------------+------------+-----------+----------------+
|          1 | John       | Doe       | 1995-03-10     |
|          2 | Alice      | Smith     | 1998-07-15     |
|          3 | Bob        | Johnson   | 1993-11-25     |
+------------+------------+-----------+----------------+
```

### 위와 같이 데이터 넣기
```
INSERT INTO students (student_id, first_name, last_name, date_of_birth)
VALUES
(1, 'John', 'Doe', '1995-03-10'),
(2, 'Alice', 'Smith', '1998-07-15'),
(3, 'Bob', 'Johnson', '1993-11-25');
```

# COURSES TABLE
```
+-----------+--------------------------------+----------------+
| course_id | course_name                    | professor_name |
+-----------+--------------------------------+----------------+
|       101 | Introduction to SQL            | Dr. Anderson   |
|       102 | Advanced Python Programming    | Prof. Martinez |
|       103 | Data Structures and Algorithms | Dr. Davis      |
+-----------+--------------------------------+----------------+
```

### 위와 같이 데이터 넣기
```
INSERT INTO courses (course_id, course_name, professor_name)
VALUES
(101, 'Introduction to SQL', 'Dr. Anderson'),
(102, 'Advanced Python Programming', 'Prof. Martinez'),
(103, 'Data Structures and Algorithms', 'Dr. Davis');
```

# ENROLLMENTS TABLE
```
+---------------+------------+-----------+------------------+
| enrollment_id | student_id | course_id | enrollment_date  |
+---------------+------------+-----------+------------------+
|             1 |          1 |       101 | 2023-01-15       |
|             2 |          2 |       102 | 2023-02-20       |
|             3 |          3 |       103 | 2023-03-25       |
|             4 |          1 |       102 | 2023-02-22       |
|             5 |          2 |       103 | 2023-03-28       |
+---------------+------------+-----------+------------------+
```

### 위와 같이 데이터 넣기
```
INSERT INTO enrollments (enrollment_id, student_id, course_id, enrollment_date)
VALUES
(1, 1, 101, '2023-01-15'),
(2, 2, 102, '2023-02-20'),
(3, 3, 103, '2023-03-25'),
(4, 1, 102, '2023-02-22'),
(5, 2, 103, '2023-03-28');
```

MySQL 공부 자료
```json
{
  "SQLStudyList": [
    {
      "주제": "SQL 소개",
      "세부주제": [
        "데이터베이스 이해",
        "기본 SQL 구문",
        "공통 데이터 유형"
      ],
      "자료": [
        {"유형": "책", "제목": "Do it! SQL 입문", "저자": "강성욱"},
        {"유형": "온라인 강의", "제목": "SQL 기초", "플랫폼": "인프런"}
      ]
    },
    {
      "주제": "데이터베이스 디자인",
      "세부주제": [
        "Entity-Relationship (ER) 모델링",
        "정규화",
        "인덱싱과 최적화"
      ],
      "자료": [
        {"유형": "책", "제목": "혼자 공부하는 SQL", "저자": "한빛미디어"},
        {"유형": "비디오 튜토리얼", "제목": "데이터베이스 디자인 Best Practices", "플랫폼": "YouTube"}
      ]
    },
    {
      "주제": "고급 SQL 쿼리",
      "세부주제": [
        "조인과 유니온",
        "서브쿼리",
        "집계 함수"
      ],
      "자료": [
        {"유형": "온라인 강의", "제목": "고급 SQL 마스터", "플랫폼": "Udemy"},
        {"유형": "블로그", "제목": "SQL 조인 마스터하기", "URL": "https://hongong.hanbit.co.kr/sql-%ea%b8%b0%eb%b3%b8-%eb%ac%b8%eb%b2%95-joininner-outer-cross-self-join/"}
      ]
    },
    {
      "주제": "데이터베이스 관리",
      "세부주제": [
        "사용자 관리",
        "백업과 복구",
        "성능 튜닝"
      ],
      "자료": [
        {"유형": "책", "제목": "SQL 서버 관리", "저자": "이영희"},
        {"유형": "포럼", "제목": "DBA 스택 익스체인지", "URL": "https://dba.stackexchange.com/"}
      ]
    }
  ]
}
```
