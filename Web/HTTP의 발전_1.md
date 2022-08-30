# HTTP의 발전_1.md

### HTTP란?
HTTP(Hyper Text Transfer Protocol)는 텍스트 기반의 통신 규약으로 인터넷에서 데이터를 주고받을 때 사용되는 프로토콜이다.
HTTP는 여러 버전을 통해 발전해왔다.

  
### HTTP 1.0 vs HTTP 1.1   


### 1) 연결 지속성(Persist Connection)
![image](https://user-images.githubusercontent.com/108853290/187375232-cf63d830-f944-4f9f-b952-89b148ca6f14.png) 

3 wah handshake는 데이터의 신뢰성을 보장하기 위한 TCP 필수적인 요소인데 매번 연결을 확인하다보니 속도면에서 아무래도 단점을 가지고 있다.
1.0 버전에서는 각 요청마다 이런 연결과 해제를 반복해야했는데 웹이 점차 고도화되면서 이 문제점은 더욱 부각되게 되었다.    

이 문제를 해결하기 위해 1.1 부터는 **한번 연결을 맺어놓으면 끊기 전까지 연결을 유지**할 수 있도록 개선되었다. 반복되는 과정을 줄임으로서 메모리와 속도면에서 비용을 줄일 수 있게 되었다.

## 2) 파이프라이닝(Pipelining)
![image](https://user-images.githubusercontent.com/108853290/187377398-0b7866a7-8292-479d-b091-5631794dc386.png)   

1.0 버전에서는 요청에 대한 응답이 와야지 다음 요청을 보낼 수 있었다. 때문에 이전의 요청 응답이 돌아오지 않으면 뒤의 요청을 위해서는 기다려야하는 상태였다.
하지만 1.1 부터는 요청이 대한 응답이 오지 않은 상태에서도 새로운 요청을 보낼 수 있게 되었다.    
**응답에 상관없이 여러개의 요청을 보낼 수 있게 된 것**이다.
  
  
### 3) 호스트 헤더(Host Header)
![image](https://user-images.githubusercontent.com/108853290/187380957-c505d29e-a578-4454-b947-21ab8134ede7.png)     

1.0에서는 하나의 IP 주소에 하나의 도메인을 운영할 수 없어서 도메인 개수만큼 서버의 개수를 늘려야했다.   
1.1에서는 **가상 호스팅(Virtual Hosting)이 가능해 졌기 때문에 하나의 IP 주소에 여러개의 도메인을 운영**할 수 있게 되었다.   
  
    
### 4) 향상된 인증 절차 (Improved Authentication Procedure)
* proxy-authentication   
* proxy-authorization   
실제 서버에서 클라이언트 인증을 요구하는 www-authentication 헤더는 HTTP 1.0부터 지원됐으나, **서버와 클라이언트 사이에 프록시가 위치하는 경우 프록시가 사용자의 인증을 요구**할 수 있는 방법이 없었다.
그러나 위와 같은 header의 추가로 프록시가 사용자의 인증을 요구하는 게 가능해졌고, 이를 통해 인증 절차가 향상되었다.

