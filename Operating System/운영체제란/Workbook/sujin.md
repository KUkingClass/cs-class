# 워크북-운영체제란?

## **Q0. 운영체제란 무엇인가요? ✨**

## A0.

```
컴퓨터 하드웨어와 사용자 사이에서 중개자(intermediary) 역할을 하며, 
하드웨어와 소프트웨어 자원을 관리하는 프로그램 입니다.
ex) Windows, MacOS, Unix, Linux, Android, iOS ..
```

## **Q1. 운영체제의 역할은 무엇인가요? ✨**

## A1.

```
1. 컴퓨터 시스템의 자원 관리를 해줍니다.
HW 자원 - CPU time, 메모리 공간, 저장 공간, I/O 장치 등 관리
SW 자원 - 프로세스, 파일 메세지 등 관리

2. 각종 I/O 장치 및 사용자 프로그램 제어를 해줍니다.

3. 응용 프로그램(Application)들의 실행 환경을 제공합니다.

4. 다른 사용자가 데이터를 삭제하거나 중요 파일에 접근하는 등 
외부 위험으로부터 보안 및 자원의 보호를 제공합니다.

5. 각종 인터페이스를 제공하여 컴퓨터 시스템 사용자의 사용 편리성을 높일 수 있습니다.
```

## **Q2. 운영체제의 기능 중에 프로스세 관리가 있습니다. 이에 대해 간략히 설명해주세요.**

## A2.

```kotlin
하드 디스크에 많은 프로그램이 존재하고, 프로그램 실행을 위해 메인 메모리에 많은
프로세스가 존재하는데 이를 할당할 수 있는 CPU는 하나뿐이기에, CPU를 점유해야 할 프로세스를
결정하고 할당하는 것을 말합니다.
```

## **Q3. 운영체제 구조 중 커널에 대해 설명해주세요.**

## A3.

```kotlin
운영체제는 규모가 매우 큰 프로그램이므로, 
운영체제의 필요한 부분만을 메모리에 올려서 사용하게 되는데, 
이 때 항상 메모리에 올라가 있는 운영체제의 핵심 부분이 커널입니다.

커널은 하드웨어와 응용 프로그램 사이에서 인터페이스를 
제공하고, 프로세스, 메모리, 저장장치 등의 자원들을 관리하는 핵심 역할을 합니다.

또한 커널에는 시스템 콜과 드라이버라는 것이 있는데,
커널은 사용자가 시스템 콜을 통해 컴퓨터 자원을 사용할 수 있게 하는 역할을 하고,
드라이버는 오디오, 키보드 등 하드웨어를 응용 프로그램에서 사용할 수 있도록 하는 역할을 합니다.

커널은 운영체제의 핵심 구성요소이고, 커널에 파일 시스템, 텍스트 에디터, 컴파일러 등의
사용자 애플리케이션 및 유틸리티를 모두 합친게 운영체제 입니다.
```