## Workbook
## [Operating System] 시스템콜
---
### 1. 시스템콜의 역할(기능)에 대하여 설명하세요.

  
  
### 2. 시스템콜의 종류에 대하여 나열하세요.



### 3. 아래 두 가지 코드의 실행결과의 차이에 대하여 설명하세요

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

```C
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



### 4. 사용자가 새로운 시스템콜을 만드는 것이 가능한가요?
