# SQL 인젝션

응용 프로그램의 보안상의 허점을 악용해 임의의 SQL문을 실행시키는 **인젝션 공격** 기법 

## 인젝션 공격

정상적이지 않은 입력값을 프로그램에 주입하여 인터프리터가 처리하도록 하는 공격 기법이다. 인젝션 공격은 프로그램의 정상적인 실행을 방해한다. 

인젝션 공격은 고전적인 공격 방법중 하나로 이 공격으로 시스템의 데이터를 빼내거나 삭제하고 Integrity 저하, DOS 까지 가능하다. 

- Code Injection
- XSS (Cross-site Scripting)
- SQL Injection
- …

## SQL 인젝션

앞에서 말한바와 같이 SQL 인젝션은 악의적인 SQL 문을 실행시킬 수 있는 인젝션 공격의 한 종류이다. 이 공격방식을 통해 해커는 웹 어플리케이션 서버 너머의 데이터베이스 서버를 조종다. 취약점을 이용하여 사용자 인증 등의 보안 설정을 우회하고 데이터베이스에 CRUD 작업을 수행할 수 있다. 

## In-band SQL 인젝션

고전적인 SQL 인젝션 공격 방법으로 공격자가 결과를 같은 채널에서 받아볼 수 있는 공격 방식이다. 공격자가 웹 브라우저를 통해 공격하면 결과 또한 웹 브라우저에 출력된다.

예를 들어 백엔드 서버가 사용자의 인풋을 받아 다음과 같은 쿼리를 데이터베이스에 보내고 결과를 화면에 뿌려준다고 해보자.

```sql
SELECT * FROM users WHERE user_id LIKE 'current_user'
```

만약 해커가 current_user 자리에 `%'—` 를 집어넣고 서버에 요청한다면 어떻게 될까? 

```sql
SELECT * FROM users WHERE user_id LIKE '%'--'
```

mysql에서 ‘—’ (더블 대쉬) 는 주석이고 ‘%’ 는 와일드카드를 의미하므로 전체 유저의 정보를 가져올 수 있다. 

### Error-based SQL 인젝션

In-band SQL 인젝션 기법 중 하나로 일부러 SQL 에러를 발생시키는 공격 기법이다. 

```sql
SELECT * FROM users WHERE user_id = 'current_user'
```

만약 해커가 `1’` 을 집어넣고 요청한다면 실제로 데이터베이스에 보내지는 SQL은 다음과 같은 것이다.

```sql
SELECT * FROM users WHERE user_id = '1''
```

뒤에 따옴표가 하나 더 붙어서 오류 메시지가 화면에 보여진다. 이 오류 메시지에는 해당 데이터베이스가 어떤 종류인지 (MySQL인지 Oracle인지) 등의 정보가 노출되어 있다. 해커는 이 정보를 이용해 더 구체적인 다른 공격 방식을 시도해볼 수 있다.

### Union-based SQL 인젝션

In-band SQL 인젝션 기법 중 하나로 기존 쿼리에 union 명령어를 더해 악성 쿼리와의 합집합을 출력하여 정보를 획득하는 공격 기법이다. 

```sql
SELECT * FROM users WHERE user_id = 'current_user'
```

만약 해커가 `1' UNION SELECT version(),current_user()--’` 을 입력한다면 실제로 보내지는 SQL은 다음과 같다.

```sql
SELECT * FROM users WHERE user_id = '-1' UNION SELECT version(),current_user()--'
```

위 쿼리의 수행 결과는 현재 MySQL의 버전과 로그인한 계정 정보가 출력된다. 단 union 시키기 위해서는 기존 쿼리의 컬럼 수와 추가한 쿼리의 컬럼 수가 일치해야 한다.

## Blind SQL 인젝션

In-band SQLi 방식과 달리 공격을 가한 데이터베이스로부터 정보를 탈취하는 대신 SQL 수행 결과를 바탕으로 데이터베이스 서버와 어플리케이션의 동작을 관찰하는 방식이다. In-based SQLi에 비해 시간은 좀 더 걸리지만 비슷한 효과를 얻을 수 있다. 데이터베이스의 다른 정보를 얻거나 데이터베이스의 구조에 대한 정보를 얻을 수 있고 다른 SQL 인젝션 기법을 적용할 수 있을지 여부를 알 수 있다.

### Boolean-based SQL 인젝션

Blind SQL 인젝션 기법 중 하나로 boolean 연산자를 주입시키는 공격 기법이다. 주입 결과에 대한 참/거짓 패턴을 통해 데이터를 유추할 수 있다.

```sql
SELECT * FROM users WHERE user_id = 'current_user'
```

예를 들어 위 쿼리의 결과를 바탕으로 웹 서버가 참/거짓을 리턴해준다고 해보자.

```sql
SELECT * FROM users WHERE user_id = '12' AND 1=1
SELECT * FROM users WHERE user_id = '12' AND 1=0
```

위 쿼리의 `1=1` 부분과 `1=0` 부분의 결과에 따라서 전체 쿼리의 참/거짓 결과가 바뀐다. 이 부분에 원하는 서브 쿼리를 넣으면 그 결과가 참인지 거짓인지를 바탕으로 데이터베이스의 정보를 유추할 수 있다. 

```sql
SELECT * FROM users WHERE user_id = '12' AND (SELECT TOP 1 substring(name, 1, 1)
  FROM sysobjects
  WHERE id=(SELECT TOP 1 id
    FROM (SELECT TOP 1 id
      FROM sysobjects
      ORDER BY id)
    AS subq
    ORDER BY id DESC)) = 'a'
```

이 서브쿼리는 데이터베이스의 첫 번째 테이블이 `a`로 시작하는지를 탐색한다. 만약 위 서브쿼리가 참이라면 `AND 1=1` 과 동일하게 동작한다. 이 작업을 수동으로 하지 않고 스크립트를 짜서 검색한다면 아주 쉽게 모든 테이블 이름 자릿수에 대하여 모든 알파벳에 대해 브루트 포스 공격을 가할 수 있다.

### Time-based SQL 인젝션

Blind SQL 인젝션 기법 중 하나로 시간 딜레이를 일으키는 쿼리를 주입시켜 데이터를 탈취하는 방법이다. 예를 들어 

```sql
SELECT * FROM users WHERE user_id = 'current_user'
```

위 쿼리에 사용자가 `12; WAITFOR DELAY '0:0:10’` 를 주입했다고 해보자. 그러면 전체 쿼리는 

```sql
SELECT * FROM users WHERE user_id = 12; WAITFOR DELAY '0:0:5'
```

이 쿼리는 5초 후에 결과를 알 수 있다. 아까 살펴본 Boolean-based SQLi 처럼 

```sql
SELECT * FROM users WHERE user_id = 42; IF(EXISTS(SELECT TOP 1 *
  FROM sysobjects
  WHERE id=(SELECT TOP 1 id
    FROM (SELECT TOP 1 id 
      FROM sysobjects 
      ORDER BY id) 
    AS subq
    ORDER BY id DESC)
  AND ascii(lower(substring(name, 1, 1))) = 'a'))
  WAITFOR DELAY '0:0:5'
```

조건문과 조합하면 테이블이 a로 시작할 때 5초 후에 결과가 나온다. 결과가 나오기까지 딜레이 되는 시간이 얼마인지를 보고 조건문이 참이었는지 거짓이었는지를 유추할 수 있다.

## SQL 인젝션 방어

기본적으로 사용자가 입력하는 값을 SQL에 그대로 사용하지 말아야 한다. 위에서 살펴보았던 예제들은 단순하게 문자열 concatenation 연산을 통해 SQL로 사용한 경우이다. 이런 경우 인젝션 공격에 그대로 노출된다. 

### Input 검증

클라이언트 측, 서버 측 모두에서 입력값이 허용된 범위 내의 값인지 검증하는 로직을 추가한다. 

### Prepared Statement 사용

Prepared Statement를 사용할 경우 쿼리의 템플릿이 미리 만들어지고 사용자의 입력은 템플릿 안에 상수 값 형태로 값이 지정된다. 그러면 사용자가 공격 쿼리를 입력한다고 하더라도 단문 문자열이기 때문에 SQL로 인식되지 않는다.