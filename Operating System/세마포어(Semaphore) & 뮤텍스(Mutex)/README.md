# [Operating System] 세마포어(Semaphore) & 뮤텍스(Mutex)
---
## 요약
* 공유된 자원의 데이터 혹은 임계구역이 있을 때
    * `세마포어`는 여러 프로세스가 진입할 수 있음, 다른 프로세스도 lock 해제 가능
    * `뮤텍스`는 하나의 프로세스만 진입할 수 있음, 해당 프로세스만 lock 해제 가능

## 임계 구역과 동기화
* 임계구역
    * 배타적으로 읽고 쓸 수 있도록 설정한 코드
    * 요구사항 3가지
        * 상호 배제 : 한 프로세스가 임계구역을 실행 중일 때, 다른 어떤 프로세스도 임계구역을 실행 불가능
        * 진행 : 임계구역을 실행하는 프로세스가 없고 여러 프로세스들이 임계구역에 들어오고자 할 때, 유한한 시간에 하나의 프로세스만을 선택하는 임계구역에 진입시키는 올바른 결정 기법이 존재
        * 제한된 대기 : 프로세스가 임계구역의 진입의 요청과 실제 요청하기 사이에 다른 프로세스가 임계구역을 실행할 수 있는 횟수에 제한이 있음
* 동기화
    * 코드 상에 임계 구역을 설정하여 코드 상으로는 독립적이지만 진입과 진출을 순서화해서 실행 상황에서 선행제약을 만드는 것

## 뮤텍스
* 두 프로세스가 임계구역에 동시에 접근하지 못하도록 막기 위해 사용하는 객체
* 구성 - lock, unlock
    * lock : 현재 임계구역에 들어갈 권한을 얻어옴, 다른 프로세스가 임계구역을 실행 중이라면 종료할 때까지 대기
    * unlock : 현재 임계구역을 모두 사용했음을 알려서 대기 중인 프로세스가 임계구역에 진입할 수 있게 함

## 세마포어
* 뮤텍스와 달리, 진입 불가능할 때 대기 상태로 전환되고 임계구역을 떠나는 프로세스가 대기 프로세스를 준비 상태로 깨워줌
* 구성 - 정수값 S, P연산과 V연산
    * 임계구역에 들어가기 전에 P연산(try)과  임계구역에서 나올 때 V연산(increment)
    * `S > 1`이면 자원이 여러 개 있을 경우 자원과 대기자를 동시에 관리 가능 
        * 프린트를 공유하는 여러 프로세스들
    * `S == 1`이면 공유변수에 대한 뮤텍스와 동일
        * 공유변수
    * `S == 0`이면 이벤트에 의한 다중 쓰레드 간의 serialization에 사용
        * 실제 공유자원은 없음

## 문제점과 해결책
* Readers Writers Problem
    * 공유자원에 여러 프로세스가 읽고 쓸 경우, 한 프로세스가 읽거나 쓸 때 모든 프로세스가 대기하는 것은 비효율
    * Reader들은 lock을 공유, Writer들은 모두 배타적으로 실행하고 Reader 대기 큐와 Writer 대기 큐를 분리
* 우선순위 역전 문제
![image](https://user-images.githubusercontent.com/64067641/192702367-960f7e16-6f2d-4ba1-8b10-fb608cc783fe.png)

    * 높은 우선순위를 가진 프로세스가 낮은 우선순위를 가진 프로세스가 공유자원 사용을 마칠 때까지 기다려야 하는 상황
    * 우선순위 상속 프로토콜
![image](https://user-images.githubusercontent.com/64067641/192702463-5db948a7-52bc-44ed-8e9c-c6ac410e857b.png)

        * 우선순위가 높은 프로세스가 우선순위가 낮은 프로세스가 실행 중인 임계구역에서 대기 중일 때, 실행 프로세스에 높은 우선순위를 상속시킴  

## 구현
* 뮤텍스
    * Peterson's solution : 상대방에게 한 번씩 양보해줌
    ```
    int turn = 0;
    boolean flag[] = [0,0};
    
    // P0						// P1
    enter_mutex {					enter_mutex {	
        flag[i] = TRUE;					    flag[j] = TRUE;
        turn = j;					    turn = i;
        while(flag[j] && turn == j);			    while(flag[j] && turn == j);
    }						}
					    
    // 임계구역					// 임계구역
					    
    exit_mutex {					exit_mutex {
        flag[i] = FALSE;					    flag[i] = FALSE;
    }						}
    ```

    * Bakery algorithm
    ```
    choosing[i] = true;
    number[i] = max(number[0],number[1],...number[n-1]) +1 ;		// 번호표 뽑기
    choosing[i] = false;
    for ( j = 0 , j < n-1, j++ ) {
        while ( choosing[j] );						// 번호표 뽑기 대기
        while ( number[j] != 0 && (number[j],j) < (number[i],i) ); 		// 먼저 번호를 뽑은 프로세스가 끝날 때 까지 대기
    }
    number[i] = 0;
    ```
* 세마포어
    ```
    P(S) {
         S--;
         if S < 0
             // 이 프로세스를 대기 큐에 추가 (잠 듦)
     }
    
     V(S) {
         S++;
         if S <= 0
             // 대기 큐로부터 프로세스를 제거 (깨어남)
     }
    ```
