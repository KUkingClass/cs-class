# 워크북-시스템콜

### **1. 시스템콜의 역할(기능)에 대하여 설명하세요.**

```
A.
프로세스가 운영체제의 기능을 사용할 수 있도록 요청하는 것인데,

쉽게 말해 사용자가 운영체제의 커널이 제공하는 서비스를 사용하기 위한 인터페이스입니다.

```

### **2. 시스템콜의 종류에 대하여 나열하세요.**

```
- 프로세스 제어 : fork(), exit(), wait() 등
- 파일 조작 : open(), read(), write(), close() 등
- 장치 조작 : ioctl(), read(), write(),
- 정보 유지(관리) : getpid(), alarm(), sleep() 등 
- 통신 : pipe(), shm_open(), mmap() 등
- 보호 : chmod(), umask(), chown() 등
```

### **3. 아래 두 가지 코드의 실행결과의 차이에 대하여 설명하세요**

```
A.
위 코드 output
parent process -> child process

아래 코드 output
child process -> parent process

아래 코드는 parent process일 경우 wait() 시스템 콜을 호출하여
자식 프로세스가 종료될 때 까지 기다리고 자식의 종료 상태를 얻어 옵니다.

그래서 위 코드와 다르게 child process가 종료 될 때까지 대기 후 parent process가
출력되는 것입니다.

이때 자식 프로세스가 종료되면 커널은 부모 프로세스를 깨워 ready 상태로 만듭니다.
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char *argv[]) {
    int rc = fork();
    
    if (rc < 0) {
        exit(1);
    }
    else if (rc == 0) {	
        printf("child process");
    }
    else {
        printf("parent process");
    }
}

```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
int main(int argc, char *argv[]) {
    int rc = fork();
    
    if (rc < 0) {
        exit(1);
    }
    else if (rc == 0) {	
        printf("child process");
    }
    else {		
        int wc = wait(NULL)	
        printf("parent process");
    }
}

```

### **4. 사용자가 새로운 시스템콜을 만드는 것이 가능한가요?**

```
결론은, '가능하다' 입니다.

새로운 시스템콜 c 파일을 작성 후 Makefile에 등록하여 사용할 수 있습니다.
```