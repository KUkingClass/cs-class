# TOC

- [저장 프로시저(Stored PROCEDURE)](#저장-프로시저stored-procedure)
  * [저장 프로시저 (Stored Procedure)](#저장-프로시저-stored-procedure)
    + [Syntax](#syntax)
    + [장점](#장점)
    + [단점](#단점)
  * [저장 함수(Stored Function)](#저장-함수stored-function)
    + [Syntax](#syntax-1)
  * [프로시저 vs. 함수 차이점](#프로시저-vs-함수-차이점)
  * [참고](#--)

# 저장 프로시저(Stored PROCEDURE)

DB에 매번 쿼리를 날려야 할까?

실제 프로그램 개발을 할 때, 자주 쓰는 명령어들을 함수로 만드는 것처럼 DB에도 함수를 만들어놓을 순 없을까?

<u>**있다!**</u>

`함수(function)`와 `프로시저(procedure)`를 DB에 저장할 수 있다. 그리고 저장한 걸 SQL문에서 호출할 수가 있다.

## 저장 프로시저 (Stored Procedure)

저장 프로시저는 나중에 쓰거나, 자주 쓰기 위해 저장해둘 수 있는 SQL 코드이다.

따라서 특정 쿼리들을 실행해야할 때마다, 쿼리들을 실행하는 대신 저장 프로시저를 호출할 수 있게 된다.

또한 '인자'를 넘길 수 있고, 해당 인자를 바탕으로 쿼리가 동작할 수 있다.

### Syntax

![image-20221209170620667](./assets/image-20221209170620667.png)

위와 같은 테이블에 insert를 하는 프로시저를 작성해보자.

```mysql
DROP PROCEDURE IF EXISTS procedure_name;

DELIMITER $$
CREATE PROCEDURE procedure_name(
IN course_id_param varchar(20), -- IN/INOUT/OUT
IN title_param varchar(20),
IN dept_name_param varchar(20),
IN credit_param int,
OUT inserted_title varchar(20)
)
BEGIN
    INSERT INTO course (course_id, title, dept_name, credits)
        VALUES (course_id_param, title_param, dept_name_param, credit_param);
    SET inserted_title=title_param;
END$$
DELIMITER ;

CALL procedure_name('CS-505', '우왕', '컴퓨터공학부', 5, @title);
SELECT @title;

SHOW CREATE PROCEDURE procedure_name;
```

- 결과

  1. `CALL` 결과

     ![image-20221209175751922](./assets/image-20221209175751922.png)

  2. `SELECT @title`
  
     ![image-20221209171344836](./assets/image-20221209171344836.png)
  
  3. PROCEDURE 조회
  
  
    | Procedure | sql\_mode | Create Procedure | character\_set\_client | collation\_connection | Database Collation |
    | :--- | :--- | :--- | :--- | :--- | :--- |
    | procedure\_name | ONLY\_FULL\_GROUP\_BY,STRICT\_TRANS\_TABLES,NO\_ZERO\_IN\_DATE,NO\_ZERO\_DATE,ERROR\_FOR\_DIVISION\_BY\_ZERO,NO\_AUTO\_CREATE\_USER,NO\_ENGINE\_SUBSTITUTION | CREATE DEFINER=\`root\`@\`localhost\` PROCEDURE \`procedure\_name\`\(<br/>IN course\_id\_param varchar\(20\), -- IN/INOUT/OUT<br/>IN title\_param varchar\(20\),<br/>IN dept\_name\_param varchar\(20\),<br/>IN credit\_param int,<br/>OUT inserted\_title varchar\(20\)<br/>\)<br/>BEGIN<br/>    INSERT INTO course \(course\_id, title, dept\_name, credits\)<br/>        VALUES \(course\_id\_param, title\_param, dept\_name\_param, credit\_param\);<br/>    SET inserted\_title=title\_param;<br/>END | utf8mb4 | utf8mb4\_general\_ci | utf8\_general\_ci |


위 프로시저처럼 IN, OUT, INOUT 파라미터를 모두 사용할 수 있다.

- 참고

  - DELIMITER?

    저장 프로시저도 sql문으로 만들어지고, 프로시저 내부에서 사용되는 sql도 sql문이다. 따라서 프로시저 내부 sql을 세미콜론(;)으로 문장을 끝맺으면, 저장 프로시저도 sql이므로 저장 프로시저가 완료되지 않았음에도 실행될 수가 있다. 따라서 저장 프로시저를 만들때까지 구분자를 다른 구분자(예: 위처럼 $$)로 바꾼 후에 생성이 끝난 후 다시 구분자를 원래대로 되돌린다.

### 장점

1. SQL 성능 향상

   저장 프로시저를 처음에 실행하면 최적화, 컴파일 단계를 거쳐 결과가 캐시에 저장되게 된다. 그 후에 저장 프로시저를 실행하면 캐시된 걸 가져와서 사용하므로 실행속도가 빨라진다.

   따라서 일반 SQL보다 성능상 좋다.

   > 이것도 매번 성능상 좋은 것은 아니다. 저장 프로시저를 처음 실행할 때 최적화 단계를 수행하므로, 이후에 파라미터에 따라 다르게 최적화하는 것이 더 유리할 때에도 기존에 최적화하여 컴파일한 방식대로 프로시저가 동작하게 된다. 대신 이 때는 다시 컴파일할 수가 있다. 프로시저를 실행할 때마다 다시 컴파일할 수 있게 설정할 수도 있다.

3. 보안 강화

   사용자에게 테이블에 권한을 주는 게 아니라 저장 프로시저에만 접근 권한을 주게 해서 보안을 강화할 수 있다. 실무에서는 실제로 개발자에게 테이블이 아닌 저장 프로시저 권한만 주는 방식을 많이 사용한다고 한다.

4. 네트워크 부하 감소

   클라이언트에서 서버로 그냥 쿼리할 때, 쿼리의 모든 텍스트가 전송될 경우 네트워크에 큰 부하가 걸리게 된다. 하지만 저장 프로시저를 이용하면 저장프로시저의 이름, 매개변수 등만 전송하면 되므로 부하를 줄일 수 있다.

5. 개발 업무 구분 가능

   애플리케이션 개발 조직 + DBMS 관련 코드를 개발하는 조직이 따로 있다면 DBMS 개발하는 조직에서는 데이터베이스 관련 처리하는 저장 프로시저를 만들어 API처럼 제공하고, 애플리케이션 개발자는 저장 프로시저를 호출해서 사용하는 방식으로 역할을 구분해서 개발할 수 있다.

6. 유지보수 및 재활용하기 좋음

   SQL문 로직이 달라지면 수정해야할 때 프로그래밍 언어 내의 SQL문을 건드리는 게 아니라 저장 프로시저만 수정하면 되므로 유지보수 측면에서 유리해진다.

   또 한번 저장 프로시저를 생성하면 특정 애플리케이션이 아닌 여러 곳에서도 실행이 가능하고, 언제든 실행이 가능하므로 재활용 측면에서 매우 좋다.

   > 코드 내에서 sql문을 그냥 작성하는 게 버전 관리가 돼서 유지 보수가 나을 수도 있지 않을까요? :thinking:

### 단점

1. 문자나 숫자 연산의 경우 처리 성능이 낮음

   문자나 숫자 연산에서는 프로그래밍 언어보다 느린 성능을 보여준다.

2. 디버깅이 어려움

3. DB 확장이 매우 힘듦

   서버수를 늘려야할 때, DB 수를 늘리는 게 더 어렵다. 서비스 확장을 위해 서버 수를 늘릴 경우 DB 수를 늘리는 것보다 웹 애플리케이션 서버의 수를 늘리는 게 더 효율적이므로 대부분 개발시에는 DB에는 최소의 부담만 주고 대부분 로직은 웹 애플리케이션 서버에서 처리할 수 있게 한다.

4. 버전 컨트롤이 지원되지 않음

5. 호환성

   복잡한 저장 프로시저는 다른 데이터베이스들과 호환되지 않을 수 있다.

## 저장 함수(Stored Function)

프로시저와 상당히 유사하지만, 형태와 사용 용도에 차이가 있다.

프로시저처럼 역시 처음 실행할 때 컴파일된다.

### Syntax

```mysql
DROP FUNCTION IF EXIST sum_func;

DELIMITER $$
CREATE FUNTION sum_func(value1 INT, value2 INT)
	RETURN INT
BEGIN
	RETURN value1 + value2;
END $$
DELIMITER ;

SELECT sum_func(100, 200);
```

## 프로시저 vs. 함수 차이점

| 함수                                                         | 프로시저                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 파라미터의 IN, OUT을 사용할 수 없고 모두 입력 파라미터로 사용된다. | 파라미터의 IN, OUT, INOUT이 모두 가능하다.       |
| 반환할 값의 데이터 형식을 지정하고, (RETURN 타입) 본문에서 하나의 값을 반환해야 한다. | 별도의 반환 구문이 없고 여러개의 OUT도 가능하다. |
| SELECT로 호출                                                | CALL로 호출                                      |
| <u>**기본적으로 DML(INSERT/UPDATE/DELETE)문 사용 불가**</u>하다. 꼭 필요하다면 BEGIN 상단에 'pragma autonomous_transaction;'을 추가해야 한다. | 가능!                                            |
| 클라이언트에서 실행되므로 프로시저보다는 느리다.             | 서버에서 실행돼서 더 빠르다.                     |

- 프로시저: 비즈니스 로직을 기술하여 업무 처리를 직접 하는 용도로 많이 사용된다.

  (예: 로그인을 위해 아이디와 비밀번호를 입력받고 해당 회원의 id를 조회하는 것처럼)

- 함수: 로직을 도와주는 목적으로 사용된다.

  (예: 매번 날짜 표시를 특정 형태로 변환해야 한다면 함수로 만들어서 재사용)

-----

## 참고

- https://wildeveloperetrain.tistory.com/132
- https://heestory217.tistory.com/18
- https://devkingdom.tistory.com/323
- https://dev.mysql.com/doc/refman/5.7/en/drop-procedure.html
- https://runcoding.tistory.com/31

- https://stackoverflow.com/questions/20495927/stored-procedure-vs-functions-compilation-and-performance-difference

- https://velog.io/@devjooj/Mysql-Function%EA%B3%BC-Procedure-%EC%B0%A8%EC%9D%B4
