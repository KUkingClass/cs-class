# SQL - JOIN

## 예시 테이블

- `course`

![image-20221117122522246](./assets/image-20221117122522246.png)

- `prereq`

![image-20221117122531879](./assets/image-20221117122531879.png)

```sql
CREATE DATABASE join_db;

USE join_db;

CREATE TABLE course(
    course_id varchar(20),
    title varchar(20),
    dept_name varchar(20),
    credits int,
    PRIMARY KEY (course_id)
);

INSERT INTO course values('CS-101', '데이터베이스', '컴퓨터공학부', 3);
INSERT INTO course values('CS-103', '데이터사이언스개론', '컴퓨터공학부', 3);
INSERT INTO course values('CS-105', '자료구조', '컴퓨터공학부', 3);
INSERT INTO course values('CS-201', '알고리즘', '컴퓨터공학부', 3);
INSERT INTO course values('CS-303', '컴퓨터공학세미나', '컴퓨터공학부', 1);

CREATE TABLE prereq(
    course_id varchar(20),
    prereq_id varchar(20),
    PRIMARY KEY (course_id, prereq_id)
);

INSERT prereq values('CS-201', 'CS-101');
INSERT prereq values('CS-201', 'CS-105');
INSERT prereq values('CS-204', 'CS-105');
```

# INNER JOIN

- 교집합으로, 기준 테이블과 join 테이블의 중복된 값을 보여준다.

![img](https://camo.githubusercontent.com/a8fc07a00af9d97c2898104cb7881a0519983ee570fdb711aed5dd6ee318b016/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65392e75662e746973746f72792e636f6d253246696d61676525324639393739394633453541383134384437303336363539)

```sql
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c join prereq p on c.course_id = p.course_id;
```

![image-20221117122954379](./assets/image-20221117122954379.png)



# LEFT OUTER JOIN

- 왼쪽 테이블을 기준으로 join해서, 왼쪽 테이블에 모든 데이터가 나타난다.

![img](https://camo.githubusercontent.com/c76a34d9927d99d7def46c2839694677d160586ca2af3eff32d98fa2ae969568/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65362e75662e746973746f72792e636f6d253246696d61676525324639393745374634313541383134393035303746303237)

![image-20221117123338920](./assets/image-20221117123338920.png)

```sql
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c LEFT JOIN prereq p on c.course_id = p.course_id
```

# RIGHT OUTER JOIN

![img](https://camo.githubusercontent.com/371a3f188280420a933172a212f74285204b85837603ae3cb973c77eb66be74d/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532352e75662e746973746f72792e636f6d253246696d61676525324639393834434533353541383134393138304142443144)

![image-20221117123427343](./assets/image-20221117123427343.png)

```sql
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c RIGHT JOIN prereq p on c.course_id = p.course_id;
```

# FULL OUTER JOIN

- 합집합으로, join에서 빠진 데이터도 null을 이용해서 나타낸다. 즉 모든 데이터가 검색된다.

![img](https://camo.githubusercontent.com/8b69d9df60427a56c5ffd62ad4d9468150dc645331e15ce27ad22e09c71d09bb/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532342e75662e746973746f72792e636f6d253246696d61676525324639393139354633343541383134393339314245304333)

![image-20221117123456672](./assets/image-20221117123456672.png)

```sql
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c LEFT JOIN prereq p on c.course_id = p.course_id
UNION
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c RIGHT JOIN prereq p on c.course_id = p.course_id;
```

# CROSS JOIN

- 모든 경우를 표현해준다. 따라서 (1번 테이블 row 개수)*(2번 테이블 row 개수) 만큼 모두 나타난다.

![img](https://camo.githubusercontent.com/c8170fd119eac82de056d7b1659824b1d398627fa09cccb70553becd4906d146/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6531302e75662e746973746f72792e636f6d253246696d61676525324639393346344534343541384132443238314143363642)

![image-20221117123646045](./assets/image-20221117123646045.png)

```sql
SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c CROSS JOIN prereq p;

SELECT c.course_id, title, dept_name, credits, p.prereq_id
FROM course c, prereq p;
```

# SELF JOIN

- 자기 자신과 자기 자신을 조인한다.
- 자신이 갖고 있는 컬럼을 변형시켜 활용할 때 활용한다.

![img](https://camo.githubusercontent.com/3600303a038c6cc6f6189738e96de0f791673b542f84c1895afa9b32a4fb6208/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532352e75662e746973746f72792e636f6d253246696d61676525324639393334314433333541384133363344303631344538)

![image-20221117123835182](./assets/image-20221117123835182.png)

```sql
SELECT *
FROM course c1, course c2;
```







