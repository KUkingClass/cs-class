# [Kotlin] Composition

## Composition Overview

> *상속보다는 컴포지션을 사용하라*
> 

- **‘is-a’** 관계를 만들기 위한 방법 → 상속(Inheritance)!
    - 단순히 코드 재사용 및 공통 속성 추출을 위해 상속을 사용한다?

<aside>
💡 컴포지션으로 **‘has-a’** 관계를 만들 수 있다.

</aside>

## 간단한 method 재사용

- 프로그레스 바(progress bar)를 로직 처리 전에 출력 & 로직 후에 숨기는 **유사한 동작을 하는 두 클래스**

```kotlin
class ProfileLoader {
		
		fun load() {
				// 1. 프로그레스 바 보여주기
				// 2. 프로필 가져오기
				// 3. 프로그레스 바 숨기기
		}
}

class ImageLoader {

		fun load() {
				// 프로그레스 바 보여주기
				// 이미지 가져오기
				// 프로그레스 바 숨기기
		}
}
```

- 상속으로 처리한다면,,

```kotlin
abstract class LoaderWithProgress {
		
		fun load() {
				// 1. 프로그레스 바 보여주기
				loadData() // 2. 데이터 가져오기
				// 3. 프로그레스 바 숨기기
		}

		abstract fun loadData()
}

class ProfileLoader: LoaderWithProgress() {
		
		override fun loadData() {
			// 프로필 가져오기
		}
}

	class ImageLoader: LoaderWithProgress() {
		
		override fun loadData() {
			// 이미지 가져오기
		}
}
```

- 부모 클래스는 1개
    - 많은 메소드를 갖는 거대한 BaseXXX 클래스
    - 깊고 복잡한 계층 구조

- 상속은 클래스의 모든 것을 가져옴
    - 불필요한 함수까지 갖게됨
    
    - 인터페이스 분리 원칙 (ISP)

- 이해하기 어려운 코드
    - 매번 부모 클래스를  확인

### composition

<aside>
💡 객체를 필드(프로퍼티)로 갖는다.

</aside>

- 변수로 선언된 객체에서 메소드 호출가능
    - 코드의 재사용성⬆️

```kotlin
class Progress {
		fun showProgress { /* */ }
		fun hideProgress { /* */ }
}

class ProfileLoader {
		**val progress = Progress()**
		
		fun load() {
				progress.showProgress()
				// 프로필 가져오기
				progress.hideProgress()
		}
}

class ImageLoader {
		**val progress = Progress()**
		
		fun load() {
				progress.showProgress()
				// 이미지 가져오기
				progress.hideProgress()
		}
}
```

- Progress 객체를 모든 클래스에서 필드로 가지고 있어야 하는 점
    - 선언부만 읽고서 코드 실행을 예측 가능
    - 프로그레스 바를 유연하게 사용 가능

- 이미지를 읽어들이고 나서 다이얼로그를 출력해야 한다면?

```kotlin
class ImageLoader {
		private val progress = Progress()
		private val finishedDialog = FinishedDialog()

		fun load() {
				progress.showProgress()
				// 이미지 가져오기
				progress.hideProgress()

				**finishedDialog.show()**
		}
}
```

- progress와 dialog 기능을 가지는 슈퍼 클래스
    - 2개의 서브 클래스에서만 dialog 띄우는 기능 필요
- 불필요한 기능을 어떻게 처리?

<aside>
💡 파라미터가 있는 생성자를 사용하자

</aside>

```kotlin
**abstract class DataLoader(val showDialog: Boolean)** {
		fun load() {
				// 프로그레스 바 보여주기
				loadData()
				if (showDialog) {
						// 다이얼로그 출력
				}
		}

		abstract fun loadData()
}

class ProfileLoader: DataLoader(showDialog = true) {
	
		override fun loadData() {
				// 프로필 읽어오기
		}
}

class ImageLoader: DataLoader(showDialog = false) {
	
		override fun loadData() {
				// 이미지 읽어오기
		}
}
```

- 서브클래스 내부적으로 불필요한 기능을 가짐 (showDialog)

### 정리

- 컴포지션은 안전하다.
    - 다른 클래스(super)의 내부적인 구현에 의존하지 않음
    - 외부에서 관찰되는 동작에만 의존
- 컴포지션은 유연하다.
    - 상속은 한 클래스만을 대상으로 하지만, 컴포지션은 여러 클래스를 대상으로 함
    - 컴포지션은 필요한 것만 받을 수 있음
    - 슈퍼 클래스의 변경 → 서브 클래스의 변경과 같은 영향 줄임
- 컴포지션은 명시적이다.
    - 메소드가 어디서 왔는지 확실히 알 수 있음 (↔ 상속의 this 키워드, 리시버 지정)
- 컴포지션은 번거롭다.
    - 객체를 명시적으로 사용 → 대상 클래스에 일부 기능을 추가한다면, 이를 포함하는 객체의 코드 또한 변경해야 함

> 상속은 언제 사용하는게 좋을까?
> 
- 명확한 ‘is-a’ 관계
    - 슈퍼클래스를 상속하는 모든 서브클래스는 슈퍼클래스로도 동작할 수 있어야 함 (리스코프 치환 원칙)

## Decorator Pattern

```
특정 클래스를 Wrapper 클래스로 감싸서 기능을 덧씌운다
```

- Composition을 통해 특정 객체를 소유
- 새 클래스의 메서드 바디에서, Composition을 통해서 가져온 메서드를 호출하도록 Forwarding

- interface

```kotlin
abstract class Beverage(
    open val name: String = "제목 없음"
) {

    abstract fun cost(): Int
}
```

- implements

```kotlin
class Espresso: Beverage(name = "에스프레소") {

    override fun cost() = 1000
}
```

- 재료
    - Wrapper (Decorator)

```kotlin
class SteamMilk(val beverage: Beverage): Beverage() {
		
    override fun cost() =  1000 + beverage.cost()

    override val name: String
        get() = "${beverage.name}, 스팀밀크"

}

class Mocha(val beverage: Beverage): Beverage() {

    override fun cost() = 1000 + beverage.cost()

    override val name: String
        get() = "${beverage.name}, 모카"

}

class Soy(val beverage: Beverage): Beverage() {

    override fun cost() =  1500 + beverage.cost()

    override val name: String
        get() = "${beverage.name}, 두유"

}

class Whip(val beverage: Beverage): Beverage() {

    override fun cost() =  1000 + beverage.cost()

    override val name: String
         get() = "${beverage.name}, 휘핑크림"

}
```

- caller

```kotlin
fun main() {
		//에스프레소 주문
    val espresso: Beverage = Espresso()
    println("${espresso.name} $${espresso.cost()}")

    // 에스프레소에 스팀밀크 + 모카 + 휘핑 주문
    var caffeMocha: Beverage = Espresso()
		caffeMocha = SteamMilk(caffeMocha)
    caffeMocha = Mocha(caffeMocha)
    caffeMocha = Whip(caffeMocha)
    println("${caffeMocha.name} $${caffeMocha.cost()}")

    // 에스프레소에 스팀밀크 + 소이 주문
    var soyLatte: Beverage = Espresso()
    soyLatte = SteamMilk(soyLatte)
    soyLatte = Soy(soyLatte)
    println("${soyLatte.name} $${soyLatte.cost()}")
}
```