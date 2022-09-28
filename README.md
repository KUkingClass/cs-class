# cs-class
cs 요리하기 🥣

참고: https://github.com/gyoogle/tech-interview-for-developer

## 스터디 방식
- 매주 과목을 정한다
- 과목의 주제별로 담당자를 정한다
- 쭉 공부 후에, `자신의 주제는 정리`해서 마크다운으로 정리한 것과 자신의 주제에서 질문을 뽑아 만든 `질문지`을 이슈를 태그하여 커밋한다.
- `질문지`를 올릴 때 이슈를 생성해서 태그하여 올린다. ([이슈 예시](https://github.com/KUkingClass/cs-class/issues/2))

  ➡️ 질문지는 스터디 전에 올려도 되고 스터디 동안 나온 질문들 합쳐서 스터디 후에 올려도 되고 상관 없습니다~!

- 저장소에 올린 후에 이슈 체크박스에 체크한다.
- 매주 만나서 정리한 걸 `발표`한다.
- 다음 만나기 전까지 `질문지를 다 채우고 위에서 생성한 이슈를 태그하여 PR로 제출`한다. ([PR 예시](https://github.com/KUkingClass/cs-class/pull/3))
- `주제 담당자가 확인`해서 Merge 해준다.

### 제출
- 파일 구조
  - [CS명](https://github.com/KUkingClass/cs-class/tree/main/CS%EB%AA%85) 폴더처럼, 각 과목에 주제명 으로 폴더를 만든 후에, 정리한 내용은 README.md로 올린다.
  - 질문지는 주제명 폴더 내에 Workbook폴더에 README.md로 올린다.
  - 제출 시에는 Workbook 폴더에 자신의 이름.md로 올린다.
- 커밋 
  - [CS명/정리] 주제 (#이슈) 로 커밋한다.
  - 예) [OS/정리] 운영체제란 (#1)
  - 워크북 올릴 시엔 [CS명/워크북], 워크북 제출할 때는 [CS명/워크북 제출]
  
## 📌 Computer Science

- ### Operating System
  - [운영체제란](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EB%9E%80) [@mimwin](https://github.com/mimwin)
  - [프로세스 vs 스레드](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20vs%20%EC%8A%A4%EB%A0%88%EB%93%9C) [@janghoosa](https://github.com/janghoosa)
  - [프로세스 주소 공간](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EC%A3%BC%EC%86%8C%20%EA%B3%B5%EA%B0%84) [@LifesLike](https://github.com/LifesLike)
  - [인터럽트(Interrupt)](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8) [@sujin-kk](https://github.com/sujin-kk)
  - [시스템 콜(System Call)](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%BD%9C) [@Lee-Jiseung](https://github.com/Lee-Jiseung)
  - [PCB와 Context Switching](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/PCB%20%26%20Context%20Switching) [@goldggyul](https://github.com/goldggyul)
  - IPC(Inter Process Communication) [@janghoosa](https://github.com/janghoosa)
  - [CPU 스케줄링](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/CPU%20스케줄링) [@LifesLike](https://github.com/LifesLike)
  - 데드락(DeadLock) [@goldggyul](https://github.com/goldggyul)
  - [Race Condition](https://github.com/KUkingClass/cs-class/tree/main/Operating%20System/Race%20Condition) [@mimwin](https://github.com/mimwin)
  - 세마포어(Semaphore) & 뮤텍스(Mutex) [@Lee-Jiseung](https://github.com/Lee-Jiseung)
  - 페이징 & 세그먼테이션 (PDF) [@sujin-kk](https://github.com/sujin-kk)
  - 페이지 교체 알고리즘 
  - 메모리(Memory)
  - 파일 시스템

- ### Network
  - [OSI 7 계층](https://github.com/KUkingClass/cs-class/tree/main/Network/OSI7%EA%B3%84%EC%B8%B5) [@sujin-kk](https://github.com/sujin-kk)
  - [TCP 3 way handshake & 4 way handshake](https://github.com/KUkingClass/cs-class/tree/main/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake) [@goldggyul](https://github.com/goldggyul)
  - [TCP/IP 흐름제어 & 혼잡제어](https://github.com/KUkingClass/cs-class/tree/main/Network/TCP:IP%20%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20%26%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4) [@janghoosa](https://github.com/janghoosa)
  - [UDP](https://github.com/KUkingClass/cs-class/tree/main/Network/UDP) [@LifesLike](https://github.com/LifesLike)
  - [대칭키 & 공개키](https://github.com/KUkingClass/cs-class/tree/main/Network/%EB%8C%80%EC%B9%AD%ED%82%A4%20%26%20%EA%B3%B5%EA%B0%9C%ED%82%A4) [@mimwin](https://github.com/mimwin)
  - [HTTP & HTTPS](https://github.com/KUkingClass/cs-class/tree/main/Network/HTTP%20%26%20HTTPS) [@Lee-Jiseung](https://github.com/Lee-Jiseung)
  - TLS/SSL handshake
  - 로드 밸런싱(Load Balancing)
  - Blocking,Non-blocking & Synchronous,Asynchronous
  - Blocking & Non-Blocking I/O
  
  
