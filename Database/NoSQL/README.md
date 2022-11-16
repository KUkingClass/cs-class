# NoSQL

## What is NoSQL?

```
보통 "비관계형" 데이터베이스라고 지칭.
또 SQL을 사용하지 않는 데이터베이스라고도 말함.(not only SQL Database)

관계형 데이터베이스와는 다른 방식으로 데이터를 저장하는 데이터베이스
```

---

## NoSQL의 데이터 관리 방식

```
예) 도서 데이터베이스를 구축해보자.
```

- `관계형 데이터베이스`
    - 데이터가 별도의 테이블화 되어 보관된다.
    - pk, fk 등 다른 테이블과의 관계또한 존재할 것이다.
        - Book 테이블에는 `id`, `title`, `author(id)` 등의 값을 가질 수 있을 것이다.
        - Author 테이블에는 `id`, `저자이름` 등 값이 있을 수 있다.
    - 테이블과 테이블 사이의 ‘**참조 무결성**’을 지키는 것이 중요하다.
    - **저장되는 레코드가 (비교적) 변경이 없이 유지될 것이라 예상하고 데이터베이스를 구축하였다.**
    
- `NoSQL 데이터베이스`
    - 도서 데이터가 `JSON` 형식으로 저장된다.
    
    ```json
    {
    	id: 1,
    	title: "해리포터와 비밀의 방",
    	author_id: 14
    }
    ```
    
    - NoSQL 데이터베이스 구축의 시작은 ‘**특정한 상황**’을 염두에 두고 설계해야 한다는 점이다.
    - 기본적인 도서 데이터 이외에도 특정한 도서 데이터를 언제든 통합할 수 있어야 한다.
        - 특정 도서 데이터에는 `series_number`  값이 존재할 수 있다.
    
    ```json
    {
    	id: 1,
    	title: "해리포터와 비밀의 방",
    	author_id: 14,
    	series_number: 2
    }
    ```
    

---

## NoSQL 데이터베이스 유형

### key-value

- 각 항목에 key-value 쌍이 포함되어 있는 가장 간단한 유형의 데이터베이스.
    - 보통 key를 참조하여 검색이 가능 → 쿼리 수행이 간단함
- 대량의 데이터를 저장해야하지만 **검색을 위해 복잡한 쿼리가 필요 없을 때** 적합.
- 장바구니 같은 일시적인 데이터 추적, 이미지나 오디오 파일
- Redis, DynanoDB, Berkely

<img width="315" alt="Untitled" src="https://user-images.githubusercontent.com/85485290/202276345-1eb7d71b-3ff9-49ac-ac8a-e7481d724e1b.png">

### document

- `JSON`과 유사 형식의 ‘문서’에 데이터를 저장
    - key-value 데이터베이스와의 중요한 차이는 값을 ‘문서’로 저장한다는 것
    - 문서 == semi-structured entity (Json, XML 등)
- 각 문서마다 다른 field-value 쌍을 가질 수 있음
    - value는 number, string, boolean, array, object 등 다양한 유형을 가질 수 있음.
- document 모델 형식을 사용하면 RDBM 보다 쉽게 대량의 데이터를 ‘수평적’으로 확장할 수 있음.
- 다양한 속성이 있는 데이터, 비정규화된 중첩 구조 데이터를 가질 때
- MongoDB
    - 아래 처럼 `articles` 라는 db가 존재한다고 해보자
    - mongoDB에서 모든 article을 조회하기 → `db.articles.find()`
    - writer가 “박명수”인 article 조회 → `db.articles.find( { “writer”: “박명수” } )`

```json
[
  {
    "title": "article01",
    "content": "content01",
    "writer": "박명수",
    "likes": 0,
    "comments": []
  },
  {
    "title": "article02",
    "content": "content02",
    "writer": "정준하",
    "likes": 23,
    "comments": [
      {
        "name": "Bravo",
        "message": "Hey Man!"
      }
    ]
  }
]
```

### graph

- Node와 Edge에 데이터 저장.
    - Node : 사람, 장소, 사물에 대한 정보 등이 저장
    - Edge : 노드 간의 관계에 대한 정보 저장
- SNS, 추천 서비스(엔진), 사기 탐지 등의 서비스 → 매우 긴밀하게 연결된 데이터셋을 구축해야 할때 적합
- Neo4j, Giraph, JanusGraph

<img width="400" alt="Untitled 1" src="https://user-images.githubusercontent.com/85485290/202276362-56cd9ef5-6f7c-4d0b-a466-0c506a7ec4aa.png">

### Wide Column Store (Column Family)

- Table, Row, Dynamic Column에 데이터를 저장
- **컬럼수가 많다면 관련된 컬럼을 컬렉션으로 묶을 수 있음**
- 대량의 데이터를 저장할 때 적합하며, join을 지원하지 않아서 쿼리 패턴을 예측할 수 있음
- IoT 데이터(지리적으로 분산), 사용자 프로필 데이터 등 동적 컬럼을 처리할 때, 수백만 테라바이트 정도의 대용량 데이터
- Cassandra, HBase

<img width="500" alt="Untitled 2" src="https://user-images.githubusercontent.com/85485290/202276383-3d99590a-a552-471a-92c8-2504bb46fa8c.png">

---

## 언제 NoSQL을 사용할까?

```
'높은 확장성'과 '가용성'이 주요 장점인 NoSQL 데이터베이스는
1. 실시간 서비스 2. 빅 데이터 3. 짧은 지연 및 대기시간을 제공해야 하는 서비스에 널리 사용된다.
ex) 게임, 전자상거래, SNS 서비스 등,,
```

- NoSQL 데이터베이스 → **다양한 데이터 모델**을 사용한다.
    - (요구사항) 변화에 빠르게 적응
- 애플리케이션에서 보이는 데이터와 비슷한 형식으로 저장됨  → 직관적, 이해하기 쉬움
    - API를 사용해서 데이터를 저장 또는 검색 시에 **데이터 변환이 줄어듬**✨

- `유연성`**:** NoSQL 데이터베이스는 일반적으로 유연한 데이터 모델을 제공하여 보다 빠르고 반복적인 개발을 가능하게 해줌 → 자유로운 형식으로 저장 (구조적, 반구조적, 비구조적 모든 형식의 데이터를 쉽게 처리)
- `확장성`**:** NoSQL 데이터베이스는 일반적으로 고가의 강력한 서버를 추가하는 대신, 분산형 하드웨어 클러스터를 이용해 확장하도록 설계되었음. → 트래픽 증가에 대응하고 지연시간을 줄일 수 있음
- `고성능`**:** NoSQL 데이터베이스는 ‘데이터 양이나 트래픽이 증가할 때’ 매우 좋은 성능을 보여줌!
- `고기능성`**:** NoSQL 데이터베이스는 각 데이터 모델에 맞춰 특별히 구축된 뛰어난 기능의 API와 데이터 유형을 제공.

### NoSQL이 적합하지 않은 경우

- NoSQL 데이터베이스는 일반적으로 **비정규화된 데이터에 의존**
    - 데이터 변형 및 데이터 중복을 방지하기 위해 고도로 정규화된 데이터에 의존하는 서비스(데이터가 구조적이고 일관적인 경우) → **NoSQL이 적합하지 않음‼️** ex) 금융, 회계 등 비즈니스 애플리케이션
- NoSQL 데이터베이스는 **단일 테이블에 대한 쿼리 작업에 매우 빠르고 간단하게 동작함.**
    - 그러나 쿼리의 복잡성이 높아지는 경우, 관계형 데이터베이스가 더 좋다. **‼️**
    - NoSQL 데이터베이스는 보통 복잡한 join, 하위 쿼리 및 WHERE 절에서의 중첩 질의를 제공하지 않기 때문.
- 이 둘 중에 꼭 선택할 필요 없이 많은 경우에 관계형 및 비관계형 데이터 모델을 결합해 사용하는 ‘하이브리드’ 접근 방식 사용
    - NoSQL로 다양한 데이터 유형을 처리하는 유연성을 얻고, RDBMS로 일관적인 읽기 및 쓰기를 보장 가능

---

## SQL vs NoSQL

### 차이점

RDBMS의 데이터는 `테이블`에 저장되어 있음.

- 스키마를 사전에 정의해야함 ==  모든 column과 데이터 타입(number, string,,)이 사전에 파악되어야 애플리케이션이 데이터를 데이터베이스에 작성할 수 있다는 말.
- 또한 key를 사용해 여러 테이블을 연결하는 방식으로 정보를 저장 → 여러 테이블 간의 관계가 형성.
- key를 통해 특정 row를 검색하거나 수정할 수도 있음

반대로 NoSQL 데이터베이스에서 데이터는 사전에 스키마를 정의하지 않아도 저장될 수 있음. ✨

- 작업을 진행하는 동시에 데이터를 정의하는 방식으로 빠르게 데이터를 작성하고 반복할 수 있다는 뜻.

최근까지도 관계형 데이터베이스가 가장 널리 사용되는 모델

- 그러나 데이터의 다양성, 속도, 양을 감당하기 위해서는 관계형 데이터베이스를 보완할 전혀 다른 데이터베이스가 필요.
- 이와 같은 상황 때문에 일부 영역에서 NoSQL 데이터베이스를 도입 →  '비관계형 데이터베이스'
- 신속한 **수평적 확장 능력** 덕분에 비관계형 데이터베이스는 높은 트래픽을 처리할 수 있고, 고도의 적응력이 특징

### NoSQL만의 특징

1. NoSQL은 구조화되지 않은 스토리지를 사용한다.
2. **Shading(sharding)** 을 거쳐서 ‘**수평확장**’을 지원한다. (여러 노드에 데이터를 분산하여 저장하는 방식)
    - SQL이 테이블간의 참조를 통해 수직 확장해 나간다면, NoSQL은 문서 옆에 문서,, 또 문서,, 계속해서 수평 확장이 가능
    - 수직 확장 = 집 안에 새로운 계단을 추가, 수평 확장 = 건물 옆에 새로운 건물을 추가
    

- 용어

| SQL | MongoDB | DynamoDB |
| --- | --- | --- |
| table | collection | table |
| row (행) | document | item |
| column (열) | field | attribute |
| primary key | objectId | primary key |
| index | index | secondary index |
| view | view | global secondary index |
| Nested table or object | Embedded document | map |
| Array | Array | list |

---

- 참고

[개발자가 NoSQL 데이터베이스를 선호하는 이유](https://www.oracle.com/kr/database/nosql/what-is-nosql/)

[NoSQL이란? | 비관계형 데이터베이스, 유연한 스키마 데이터 모델 | AWS](https://aws.amazon.com/ko/nosql/)
