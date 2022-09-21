# UDP
UDP란? User Datagram Protocol

![tcp-ip-layers](https://user-images.githubusercontent.com/33995823/191456087-47d2ca9e-b657-46bd-b58d-3f6d75611496.jpg)

TCP와 함께 Transport 계층을 구성하는 프로토콜

## UDP의 특징
* 비 연결형 프로토콜이다. (Connectionless)
* 신뢰성이 없다. (Unreliable)
* TCP에서 제공하던 거의 모든 기능을 제공하지 않는다.
* 가볍고 속도가 빠르다.

## TCP와의 차이점
* 데이터 전송 전 연결 단계가 없고 바로 전송을 시작한다.
	* c.f) TCP는 handshake 를 먼저 거친다.
* 헤더 크기가 8 바이트로 고정되어있다.
	* c.f) TCP는 20 ~ 60 바이트의 헤더 크기를 가진다.
* 패킷의 순서를 보장하지 않는다.
* Receiver가 제대로 받았는지 확인하지 않는다.

![tcp-header](https://user-images.githubusercontent.com/33995823/191456113-c9057bf2-1feb-4426-9ccd-7d44dc9c7412.png)

![udp-header](https://user-images.githubusercontent.com/33995823/191456131-652035f9-274c-4a74-baac-027b2697465d.png)

## UDP 사용 예시
### DNS
우리가 웹 브라우저에 www.naver.com 을 입력하고 엔터키를 누르면 바로 사이트로 이동하는것이 아니다. 해당 도메인에 실제로 연결된 IP 주소를 알아야 사이트에 접근할 수 있다. 이때 사용하는 서비스가 DNS이다. DNS가 UDP를 사용하는데에는 몇 가지 이유가 있다.

* DNS 서버가 TCP를 사용한다면 클라이언트가 주소 변환 결과를 얻는데 걸리는 시간이 늘어난다.
* DNS 서버가 TCP를 사용해서 클라이언트와 연결을 계속 맺고 있을 필요가 없다.
* DNS 질의는 대부분 아주 작은 크기이기 때문에 UDP 세그먼트 사이즈에 알맞다.

그런데 사실 DNS에서 TCP도 사용한다.  응답의 길이가 512 바이트를 초과하는 경우 또는 DNS 서버로부터 응답을 받지 못한 경우 TCP를 사용해서 다시 요청한다.

> 참고  
> DNS 응답이 512 바이트를 초과하는 경우  
>   
> DNS 서버는 일종의 분산 데이터베이스이다.  Slave 서버는 주기적으로 Master 서버에 접속하여 파일을 비교하고 최신화 하는 과정이 필요한데 이를 Zone Transfer라고 한다. 이 과정에서 모든 레코드 정보를 복사해온다.  
>   
> 또 IPv6 주소를 사용하는 경우 512 바이트를 넘길 수도 있다.  
>   
>   ![](README/dsrc-dns-security-center-faq-dns-querying.jpg.webp)  
>   
> 어떤 쿼리의 리턴은 TXT 레코드 (Site Verification, Spam Detectipn 등) 가 나가기도 한다.  
>   
>   ![](README/dsrc-dns-security-center-faq-dns-querying-spam-detection.jpg.webp)  
>   
> DNS 보안을 위한 DNSSEC이 세팅되어 있는 경우 암호 키, 암호 알고리즘 등이 포함되어 응답 크기가 커진다.  
>   
>   ![](README/dsrc-dns-security-center-faq-dns-querying-cryptographic-keys.jpg.webp)  
>   

DNS 패킷 사이즈 문제는 사실 오래전부터 나왔던 이야기이고 프로토콜의 사이즈를 키운 EDNS (Extension Mechanism for DNS) 가 제안되긴 했지만 여전히 널리 적용되지는 않았다.


### 온라인 게임
온라인 게임에서는 TCP를 사용해서 생기는 딜레이가 (비록 신뢰성은 높지만) UX에 심각한 악영향을 끼친다. 예를들어 FPS 게임에서 총알의 발사, 캐릭터의 움직임 등은 전부 UDP를 사용한다. 만약 신뢰성이 보장되어야 한다면 TCP를 사용하는것이 아니라 UDP 위에 직접 구현한다.

### 비디오/오디오 스트리밍
UDP를 사용해서 생기는 약간의 데이터 손실이 사용자에게 큰 영향을 미치지 않는다. 예를들어 초당 60 프레임으로 재생되는 영상에서 한 두 프레임에 데이터 손실이 일어난다고 해도 사용자가 알아차리기 쉽지 않다.

### HTTP/3
2022년 6월 표준화된 HTTP/3 프로토콜은 기저 프로토콜로 이전 버전까지 사용하던 TCP 프로토콜 대신 UDP를 채택했다. 

![tcp-tls](https://user-images.githubusercontent.com/33995823/191456166-8dd971dc-f1c7-4041-9c32-e0ed8dac59b6.png)

![http-request-over-quic@2x](https://user-images.githubusercontent.com/33995823/191456181-293847ad-58c0-4c48-bacd-ebbf6f083149.png)


#CS 스터디/네트워크/UDP#
