# [JAVA] Serialization(직렬화)

---

## Java Serialization

- 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터를 변환하는 기술과 바이트 데이터를 다시 객체로 변환하는 기술
- JVM의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술과 바이트 형태의 데이터를 객체로 변환해서 JVM에 상주시키는 기술
- object ↔ byte stream

## 직렬화 사용

### 조건

- primitive 타입이거나 Serializable 인터페이스를 상속받은 객체

### 방법

```java
Member member = new Member("이름", "email@email.com", 26);

byte[] serializedMember;
try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
    try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
        oos.writeObject(member);
        // serializedMember -> 직렬화된 member 객체 
        serializedMember = baos.toByteArray();
    }
}

// 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
System.out.println(Base64.getEncoder().encodeToString(serializedMember));
```

### 역직렬화 조건

- 직렬화 대상이 된 객체의 클래스가 클래스 패스에 존재해야 하며 import 되어 있어야 함
- 자바 직렬화 대상 객체는 동일한 serialVersionUID를 가지고 있어야 함

### 역직렬화 방법

```java
String base64Member = "...";
byte[] serializedMember = Base64.getDecoder().decode(base64Member);
try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)) {
    try (ObjectInputStream ois = new ObjectInputStream(bais)) {
        // 역직렬화된 Member 객체를 읽어온다.
        Object objectMember = ois.readObject();
        Member member = (Member) objectMember;
        System.out.println(member);
    }
}
```

### transient

- 직렬화하는 대상에서 제외하고 싶은 항목에 붙이는 키워드
- 역직렬화하면 null값이 들어감

### serialVersionUID

- 객체의 버전을 저장
- static final long serialVersionUID
- 지정해주지 않으면 클래스 해쉬값으로 지정해줌
- SUID가 동일하면 변수 및 메서드 추가 가능, 제거 혹은 이름 변경은 오류는 아니지만 데이터 누락
    
    그래도 자바 역직렬화는 타입에 엄격하므로 타입 변경은 불가능
    

```java
public class Parent implements Serializable {
	  String name;
		String password;
}

public class Child extends Parent {
		int age;
}
//Child{name='이름', password='1234', age=26}
```

```java
public class Parent {
	  String name;
		String password;
}

public class Child extends Parent implements Serializable {
		int age;
}
//Child{name='null', password='null', age=26}
```

```java
public class Child implements Serializable {
		int age;
		Object obj = new Object();
}
//Exception in thread "main" java.io.NotSerializableException: java.lang.Object
```

## 직렬화는 왜 사용하는 걸까

- CSV나 JSON과 같은 데이터를 문자열 형태로 확인 가능한 직렬화 방법이 있다.
- 자바 직렬화 형태의 데이터 교환은 자바 시스템 간의 데이터 교환을 위해서 존재
- CSV, JSON 말고 자바 직렬화를 쓰는 장점
    - 데이터 타입이 자동으로 맞춰짐
    - 복잡한 클래스의 객체라도 직렬화 기본 조건만 지키면 바로 직렬화, 역직렬화 가능
- 단점
    - 역직렬화 시 생기는 예외
    - JSON보다 용량이 큼
    - 자바에서만 사용

### 직렬화 응용

- JVM의  메모리에서만 상주되어있는 객체 데이터를 그대로 영속화가 필요할 때 사용, 시스템이 종료되더라도 유지되므로 네트워크로 전송도 가능
- 서블릿 세션
    - 서블릿 기반의 WAS(톰캣, 웹로직)들은 대부분 세션의 자바 직렬화를 지원
    - 세션을 메모리 위에서 운용한다면 직렬화가 필요없지만, 파일로 저장하거나 세션 클러스터링, DB 저장하는 옵션 등을 선택하게 되면 세션 자체가 직렬화되어 저장 및 전달
- 캐시
    - 자바 시스템에서 성능을 위해 캐시 라이브러리(Ehache, Redis, Memcached)를 사용
    - 캐시 할 부분을 자바 직렬화된 데이터를 저장해서 사용
    - DB를 조회해 가져온 데이터 객체가 실시간성을 요구하지 않는다면 메모리, 외부 저장소, 파일 등의 저장소를 이용해서 객체를 저장하고 동일한 요청이 오면 DB에 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답
- 자바 RMI
    - 원격 시스템 간의 메시지 교환을 위해서 사용하는 자바에서 지원하는 기술
    - IP와 포트를 이용하는 소켓통신 대신 원격의 시스템의 메소드를 로컬 시스템의 메소드인 것처럼 호출 가능
    - 이때 원격의 시스템의 메소드의 호출에 전달하는 객체를 자동으로 직렬화 시켜 사용하고 전달받은 객체는 역직렬화하여 사용

## 결론

- 자주 변경되는 클래스의 객체에는 직렬화 사용 지양
- 외부 저장소로 저장되는 데이터는 짧은 만료시간의 데이터를 제외하고는 직렬화 사용 지양
- 외부에 장기간 저장될 정보는 직렬화 사용을 지양 ⇒ 언제 예외가 발생할지 모름
    
    역직렬화에 대한 예외처리가 필요
    
## 출처
[https://techblog.woowahan.com/2550/](https://techblog.woowahan.com/2550/)
