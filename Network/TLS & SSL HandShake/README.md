# TLS/SSL HandShake

## TLS/SSL HandShake 란?
```
HTTPS에서 클라이언트와 서버간 통신하기 전, 
SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식
```

<br>

모든 SSL/TLS 연결은 **'HandShake'** 과정을 거쳐야 한다.  
- **'HandShake'** 과정은  
  - 통신을 암호화하는데 ```사용할 암호화 알고리즘과 키``` 결정  
  - 서버 확인  
  - 실제 데이터 전송을 시작하기 전 ```보안 연결```이 이루어졌는지 확인  

이러한 연결을 확인하기 위한 과정이 handshake 이다.

<br>

## TLS/SSL 클라이언트와 서버의 통신 단계
```
1. 사용할 프로토콜 버전에 동의
2. 암호화 알고리즘 선택
3. 디지털 인증서 교환하고 유효성 검사하여 서로 인증
4. 비대칭 암호화 기술을 사용하여 공유키 생성
5. SSL/TLS 는 공유키를 사용하여 대칭 암호화 방식을 사용하여 메세지 암호화
```

handshake 자체는 비대칭 암호화를 사용한다. (공개키와 개인키 별도 사용)  
하지만, 비대칭 암호화는 ```오버헤드가 높아``` 모든 보안 과정에서 사용할 순 없다. 

그래서 **공개키**는 암호 해독을 위한 암호화 및 개인키로 사용되며, 서버와 클라이언트가 각각 새로 생성한 공유키를 설정하고 교환하게 한다.  
세션 자체는 **공유키**를 사용하여 대칭 암호화를 사용하기 때문에 실제 연결에서 오버 헤드가 줄어든다. 

<br>

## 진행 순서
![image](https://user-images.githubusercontent.com/34904741/139517776-f2cac636-5ce5-4815-981d-33905283bf13.png)

<br>


1. 클라이언트는 서버에게 `client hello` 메시지를 담아 서버로 보낸다.
   이때 암호화된 정보를 함께 담는데, `버전`, `암호 알고리즘`, `압축 방식` 등을 담는다.  
   
2. 서버는 클라이언트가 보낸 암호 알고리즘과 압축 방식을 받고, `세션 ID`와 `CA 공개 인증서`를 `server hello` 메시지와 함께 담아 응답한다. 이 CA 인증서에는 앞으로 통신 이후 사용할 대칭키가 생성되기 전, 클라이언트에서 handshake 과정 속 암호화에 사용할 공개키를 담고 있다.  

3. 클라이언트 측은 서버에서 보낸 CA 인증서에 대해 유효한 지 CA 목록에서 확인하는 과정을 진행한다.  

4. CA 인증서에 대한 신뢰성이 확보되었다면, 클라이언트는 난수 바이트를 생성하여 서버의 공개키로 암호화한다. 이 난수 바이트는 대칭키를 정하는데 사용이 되고, 앞으로 서로 메시지를 통신할 때 암호화하는데 사용된다.  

5. 만약 2번 단계에서 서버가 클라이언트 인증서를 함께 요구했다면, 클라이언트의 인증서와 서버의 공개키로 암호화한 난수 바이트를 함께 보내준다.  

6. 서버는 클라이언트의 인증서를 확인 후, 난수 바이트를 자신의 개인키로 복호화 후 대칭 마스터 키 생성에 활용한다.  

7. 클라이언트는 handshake 과정이 완료되었다는 `finished` 메시지를 서버에 보내면서, 지금까지 보낸 교환 내역들을 해싱 후 그 값을 대칭키로 암호화하여 같이 담아 보내준다.  

8. 서버도 동일하게 교환 내용들을 해싱한 뒤 클라이언트에서 보내준 값과 일치하는 지 확인한다. 일치하면 서버도 마찬가지로  `finished` 메시지를 이번에 만든 대칭키로 암호화하여 보낸다.  

9. 클라이언트는 해당 메시지를 대칭키로 복호화하여 서로 통신이 가능한 신뢰받은 사용자란 걸 인지하고, 앞으로 클라이언트와 서버는 해당 대칭키로 데이터를 주고받을 수 있게 된다.  