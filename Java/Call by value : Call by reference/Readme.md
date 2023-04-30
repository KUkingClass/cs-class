# [Java] Call by value / Call by reference

함수 호출 방법은 크게 두가지가 있다.

## Call by value

> 값에 의한 호출
> 
- 인자로 받은 값을 복사하여 처리
    - 물이 찬 컵과 똑같은 물 한컵을 준비해서 사용

- 아래 코드는 swap이 되지 않는다.

```c
#include <stdio.h>

<<<<<<< HEAD
void swap(int num1, int num2){
	int temp = num1;
	num1 = num2;
	num2 = temp;
}

int main(){
	int a = 10;
	int b = 20;
	
	printf("before : %d, %d\n", a, b);
	
	swap(a, b);
	
	printf("after : %d, %d\n", a, b);
	
	return 0;
}

/*
  before : 10, 20
  after : 10, 20
*/
```

- 지역변수(a, b)와 매개변수(num1, num2)는 값이 `Stack`에 할당✨
- swap 함수를 호출하는 순간 a, b 값을 num1, num2라는 새로운 값으로 복사하는 것!
- swap 내부적으로 처리가 일어나고 아무것도 넘기지 않기 때문에 a,b 값에는 변화 X
    - 그럼 a, b에 어떻게 접근할 수 있을까?✨

- `in Stack`
    - a = 10, b = 20
    - num1 = 20, num2 = 10

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled.png)

- C에는 콜바이 레퍼런스가 없다 ^^ 고쳐!

## Call by reference

> 참조에 의한 호출
> 
- 인자로 받은 값의 주소를 참조하여 직접 처리
    - 물이 찬 컵을 직접 가져다가 사용

- a, b를 직접 참조해서 swap 해보자

```c
#include <stdio.h>

void swap(int *num1, int *num2){
	int temp = *num1;
	*num1 = *num2;
	*num2 = temp;
}

int main(){
	int a = 10;
	int b = 20;
	
	printf("before : %d, %d\n", a, b);
	
	swap(&a, &b);
	
	printf("after : %d, %d\n", a, b);
	
	return 0;
}

/*
  before : 10, 20
  after : 20, 10
*/
```

- 값이 복사되는 점은 동일 → **복사되는 값이 데이터의 주소 값✨**
- `in Stack`
    - a = 10, b = 20
    - num1 = 0x00, num2 = 0x04

num1이 가리키는 값과 num2가 가리키는 값을 바꾸겠다!✨

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%201.png)

## Java에서의 호출

- Person이라는 객체의 age라는 field 값을 변경해보자
    - p의 age 값이 바뀌었다 → call by reference ?

```java
class Example {
    public static void main(String[] args) {
        Person p = new Person(10);

        System.out.println("before : "+ p.age);

        getOlder(p);

        System.out.println("after : "+ p.age);
    }

    private static void getOlder(Person p){
        p.age++;
    }
}

/*
  before : 10
  after : 11
*/
```

### Java 메모리 구조

**Static**

**Stack**

- 메소드 내에서 정의하는 **primitive data type (int, double, byte, long, boolean 등)**
- 지역변수, 매개변수
- 해당 메소드가 호출 될 때 메모리에 할당되고 종료되면 메모리가 해제 됨

**Heap**

- 참조형 데이터
- 객체, 배열
- JVM의 Garbage Colletion에 의해 메모리 해제
- 데이터 자체는 Heap 영역에 있어도, 이 데이터를 가리키는 주소값 (참조값)은 Stack에 존재

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%202.png)

- p = new Person(10)

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%203.png)

- getOrder() 호출 후
    - p 값이 바뀐건가? 아니다!✨
    - p를 참조하는 값 (0x0004) 주소는 그대로임 → call by value

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%204.png)

```
결국 call by value인지 call by reference인지 확인하는 법 
→ 직접 객체(값)의 주소가 바뀌는지 확인해야 한다‼️
```

- 직접 p의 값(주소값)을 바꿀 수도 있을까?

```java
class Example {
    public static void main(String[] args) throws Exception{
        Person p = new Person(10);

        System.out.println("before : "+ p.age);

        changePerson(p);

        System.out.println("after : "+ p.age);
    }

    private static void changePerson(Person p){
        p = new Person(50);
    }
}

// before : ?, after : ?
```

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%205.png)

- p = new Person(50)
- 다른 인스턴스를 참조하도록 한다면?
    - main의 p (0x0004)가 0x000C로 값이 바뀌면 call by reference일 것

![Untitled](%5BJava%5D%20Call%20by%20value%20Call%20by%20reference%20ad48c9a904b841df8608bba81b68bdc0/Untitled%206.png)

- 결과는,, changeOlder 함수가 종료되도 p는 0x0004로 그대로 남는다 → **Java는 Call by value!**
=======
- == vs equals
    - `==` : 동일한 메모리 공간인지 비교
    - `equals()` : 객체의 내용 비교

```java
// 리터럴
a == b -> true
a.equals(b) -> true

// 객체
a == b -> false
a.equals(b) -> true
```

<aside>
💡 String vs StringBuffer / StringBuilder의 차이점 → **String은 Immutable(불변) 하다!**

</aside>

- str이 가리키는 값이 “hello world”로 바뀌는걸까?

```java
String str = "hello";
str = str + "world";
```

- “hello word” 라는 값을 가지는 **새로운 메모리 영역을 가리키게 된다.✨**
    - “hello”에 할당되어 있던 메모리 영역은 GC에 의해 사라지게 됨
    - 한번 생성된 객체 내부의 내용을 변화시킬 수 없고 **새로운 String 인스턴스가 생성**되므로 불변하다고 한다.

![Untitled](./assets/Untitled.png)

- 그래서 문자열 추가, 수정, 삭제 등의 연산이 빈번하게 발생하는 경우 → String을 사용하면 heap 메모리 부족을 야기,,

## StringBuffer

<aside>
💡 그래서 **mutable(가변)**한 StringBuffer / StringBuilder 도입

</aside>

- `append()`, `delete()` 등의 API로 동일 객체내의 변경이 가능하다.
- **문자열 추가, 수정, 삭제 등의 연산이 빈번한 경우**라면 StringBuffer/StringBuilder 사용

```java
StringBuffer sb = new StringBuffer("hello");
sb.append(" world")
```

- str을 가리키는 참조값은 변화하지 않는다.

![Untitled](./assets/Untitled%201.png)

- `append()` 내부 살펴보기
    - 문자열을 추가하게 되면 `len`(문자열의 길이) 만큼 문자열을 저장하는 배열의 공간을 늘려주고, 늘려준 공간에 추가 할 문자열을 넣어줌 → **값이 변경되는 가변성, 같은 주소 공간 참조✨**

```java
public AbstractStringBuilder append(String str) {
        if (str == null) {
            return appendNull();
        }
        int len = str.length();
        ensureCapacityInternal(count + len);
        putStringAt(count, str);
        count += len;
        return this;
    }
```

## StringBuilder

<aside>
💡 StringBuffer는 동기화 키워드를 지원하여 **thread-safe** 하다.

</aside>

- String 또한 Immutable하기 때문에 멀티쓰레드 환경에서 안전하다.

**StringBuilder는 동기화를 지원하지 않는다. → 단일 쓰레드에서 성능이 뛰어나다. ✨**

- 단일 쓰레드 환경에서 StringBuffer를 사용하면 성능이 매우 떨어지게 됨

- StringBuffer의 `synchronized` 키워드

![Untitled](./assets/Untitled%202.png)

## 정리

- String vs StringBuffer / StringBuilder
    - **Immutable vs Mutable**

- StringBuffer vs StringBuilder
    - **동기화 유무 (Synchronization)**

- `String` : 문자열 연산이 적고, 멀티쓰레드 환경 (Thread-safe)
- `StringBuffer` : 문자열 연산이 많고, 멀티쓰레드 환경 (Thread-safe)
- `StringBuilder` : 문자열 연산이 많고, 단일쓰레드 혹은 동기화를 고려하지 않는 경우

(참고)

- JDK 1.5 이후부터 String 객체를 사용하더라도 StringBuilder로 컴파일 되도록 변경되었음 → String을 사용해도 StringBuilder와 성능상의 차이가 없음

- **Single Statement(한 줄)**로 문자열을 + 연산하면 컴파일러가 자동으로 StringBuilder로 최적화

```java
String str = "1, " + "2, " + "3, " + ,,,;
```

- But, **반복문 내에서 문자열을 계속 더해갈 때는** 최적화 하지 못하고 String은 객체를 계속 더하게 됨 → StringBuilder를 사용해야 함

```java
String str = "";
for (int i=0; i<100; i++) {
	str += ", " + i;
}
```
>>>>>>> bb8d5c241745cd634ab6828a69b6bc17635c3993
