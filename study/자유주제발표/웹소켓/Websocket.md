# Websocket (웹소켓)
-----------------------------------
-----------------------------------
## 1. 웹소켓의 역사
### &nbsp; 1) HTTP
<img src = "data:image/png;base64,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">

#### &nbsp;&nbsp; (1) 클라이언트(웹브라우저)와 웹 서버 통신을 위한 프로토콜    
#### &nbsp;&nbsp; (2) 사용자는 서버로부터 새로운 정보를 받아보기 위해서, 반드시 새로운 URL을 요청해야 한다는 전제를 가짐
#### &nbsp;&nbsp; (3) HTTP의 실시간 통신 방식
#### &nbsp;&nbsp;&nbsp; - Polling
<img src = "https://miro.medium.com/v2/resize:fit:720/format:webp/1*YiWBVCm1Ge7LklMsOcZi2g.png" width="60%">

##### &nbsp;&nbsp;&nbsp;&nbsp; : 브라우저가 일정한 주기마다 서버에 HTTP 요청을 보내는 방식
##### &nbsp;&nbsp;&nbsp;&nbsp; : 실시간 데이터의 업데이트 주기는 예측 불가능하므로,  불필요한 요청에 따른 서버 및 네트워크 부하가 크게 늘어남
##### &nbsp;&nbsp;&nbsp;&nbsp; : 5~10초 주기로 계속 업데이트 시키는 방식
##### &nbsp;&nbsp;&nbsp;&nbsp; : time interval을 어떻게 잡냐에 따라 서버의 부하가 올라가거나 실시간성이 떨어지는 trade off 관계를 갖음
#### &nbsp;&nbsp;&nbsp; - Long Polling
<img src = "https://miro.medium.com/v2/resize:fit:720/format:webp/1*JyLiDASqEXBs3ZjvldUrEQ.png" width="60%">

##### &nbsp;&nbsp;&nbsp;&nbsp; : polling의 서버 부하를 줄이면서 실시간성을 높이기 위한 방식
##### &nbsp;&nbsp;&nbsp;&nbsp; : HTTP 요청 시 서버는 해당 연결을 바로 해제하지 않고 일정시간 대기 후 변경이 일어났을 때 바로 클라이언트에게 요청, 응답 받은 클라이언트는 바로 서버에 다시 요청을 보냄
##### &nbsp;&nbsp;&nbsp;&nbsp; : 응답이 와서 연결이 끊기면 클라이언트가 서버에 다시 요청
##### &nbsp;&nbsp;&nbsp;&nbsp; :  여러 클라이언트와 잦은 데이터 변경이 일어나면 서버의 부담이 큼
##### &nbsp;&nbsp;&nbsp;&nbsp; : 1:1 채팅 등 실시간성이 필요한 적은 수의 클라이언트와 연결되어있는 경우 사용
#### &nbsp;&nbsp;&nbsp; - Streaming
##### &nbsp;&nbsp;&nbsp;&nbsp; : long polling의 연결구축에 대한 부하를 해결하는 방식
##### &nbsp;&nbsp;&nbsp;&nbsp; : 요청에 대한 응답을 완료하지 않은 상태에서 데이터를 계속 내려받는 방식으로 응답을 받더라도 연결을 끊고 다시 request요청을 보내는 과정이 없고 계속 응답을 받아 처리
##### &nbsp;&nbsp;&nbsp;&nbsp; : 서버는 무한정 혹은 일정 시간동안 요청을 대기시키고, chunked 메시지를 이용해 응답시 연결을 계속 유지함
##### &nbsp;&nbsp;&nbsp;&nbsp; : 클라이언트에서 서버로 데이터를 보내는게 힘들어  실시간 단방향 통신으로 주로 이루어짐

<br>

### &nbsp; 2) AJAX
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnC23n%2FbtqAfngnOP9%2FJNYIXrONXHR12ahyFTL4KK%2Fimg.png">

#### &nbsp;&nbsp; (1) HTTP를 효과적으로 이용하는 기술
#### &nbsp;&nbsp; (2) 유저는 새로운 웹페이지로 이동하는 것이 아니고 동일한 웹페이지 내에서 DOM을 변경하는 것
#### &nbsp;&nbsp; (3) 여전히 HTTP로 서버와 통신 즉 클라이언트의 요청이 있고 그 다음 서버로 부터 응답 받는 상황
#### &nbsp;&nbsp; (4) 클라이언트가 서버에게 요청하는 폴링 방식 사용됨

<br><br>


## 2. 웹소켓 개념
<img src ="https://miro.medium.com/v2/resize:fit:720/format:webp/1*LIqT-pCAvTvAHsDfxc9xuQ.png">

### &nbsp; 1) Transport protocol의 일종으로 서버와 클라이언트 간의 효율적인 양방향 통신을 실현하기 위한 구조, 즉 웹에서 하나의 TCP 연결을 통해 양방향 통신을 제공하는 컴퓨터 통신 프로토콜
### &nbsp; 2) 단순 API로 구성되어 있고, 웹소켓을 이용하면 하나의 HTTP 접속으로 양방향 메시지를 자유롭게 주고받을 수 있음
### &nbsp; 3) HTTP의 반이중 (Half-Duplex) 방식이 아닌 진짜 양방향 통신인 전이중(Full-Duplex) 방식
#### &nbsp;&nbsp; (1) Half-Duplex : 양 방향 통신을 하지만 송수신을 동시에 할 수 없고, 무전기 방식처럼 해야함
#### &nbsp;&nbsp; (2) Full-Duplex : 동시에 송수신을 하며 양 방향 통신을 할 수 있음
### &nbsp; 4) TCP socket은 바이트 스트림을 이용하지만 Web socket은 UTF-8의 텍스트와 바이너리 둘 다 가능
### &nbsp; 5) HTTP 요청을 그대로 사용하므로 80, 443 포트를 그대로 사용할 수 있고, HTTP의 규격인 CORS적용, 인증 등 기존과 동일하게 이용 가능
### &nbsp; 6) 최초 접속이 일반 http요청을 이용한 handshaking으로 이루어짐
### &nbsp; 7) Statefull 
#### &nbsp;&nbsp; (1) 서버와 클라이언트가 한번 연결되면 계속 같은 라인을 사용해 통신하므로 HTTP 사용시 필요업싱 발생되는 HTTP와 TCP 연결 트래픽을 피할 수 있음
#### &nbsp;&nbsp; (2) 웹소켓은 최초접속을 제외하면 헤더 정보를 보내지 않지만, HTTP 프로토콜은 요청을 할 때마다 헤더 정보를 보내게 되므로 네트워크 비용에서는 이득
### &nbsp; 8) 브라우저는 서버가 직접 보내는 데이터를 받아들일 수 있고, 사용자가 다른 웹사이트로 이동하지 않아도 최신 데이터가 적용된 웹을 볼 수 있게 해줌, 웹페이지를 ‘새로고침’하거나 다른 주소로 이동할 때 덧붙인 부가 정보를 통해서만 새로운 데이터를 제공하는 웹서비스 환경의 빗장을 본질적으로 풀어준 셈



<br><br>


## 3. 웹소켓 동작 원리
<img src ="https://velog.velcdn.com/images%2Fwldus9503%2Fpost%2F6a3a579c-93be-44d1-ab8e-f69d7f42823c%2F6.PNG">

### &nbsp; 1) 웹소켓 동작 원리
#### &nbsp;&nbsp; (1) 웹소켓 연결을 위해 http통신을 함
#### &nbsp;&nbsp; (2) Handshake 과정
#### &nbsp;&nbsp;&nbsp; - Handshake 요청 
<img src = "https://velog.velcdn.com/images%2Fwldus9503%2Fpost%2F314ccd77-3701-4490-8c94-878057dd21a4%2F캡처.PNG" width="70%"> 

##### &nbsp;&nbsp;&nbsp;&nbsp; : 클라이언트에서 요청을 할 떄 HTTP 1.1 로 요청하고, 웹 소켓 프로토콜로 업그레이드를 해줄 것을 요청, 요청 헤더에는 소켓 버전과 소켓 비밀키 정보 등을 포함
#### &nbsp;&nbsp;&nbsp; - Handshake 응답 
<img src = "https://velog.velcdn.com/images%2Fwldus9503%2Fpost%2F94d9668c-ccd4-4dd2-aac6-1eb25838e73d%2F2.PNG" width="70%"> 

##### &nbsp;&nbsp;&nbsp;&nbsp; : 서버에서는 클라이언트로 Handshake 응답이 옴, http 1.1에서 WebSocket으로 프로토콜 업데이트가 성공되면 HTTP status 101 응답을 전송

#### &nbsp;&nbsp; (3) HTTP → 웹 소켓 프로토콜로 변경하는 프로토콜 스위칭이 이뤄짐
#### &nbsp;&nbsp; (4) 웹소켓을 위한 소켓이 생성되고 해당 소켓으로 전이중 통신을 함
### &nbsp; 2) 연결이 정상적으로 이루어진다면 서버와 클라이언트 간에 웹소켓 연결(TCP/IP기반)이 이루어지고 일정 시간이 지나면 HTTP연결은 자동으로 끊어짐 
### &nbsp; 3)웹소켓 API는 간단한 기능들만을 제공하기 떄문에 대부분의 경우 SockJS나 Socket.IO 같은 오픈 소스 라이브러리를 많이 사용하고 있으며 메시지 포맷 또한 STOMP 같은 프로토콜을 같이 이용함


<br><br>


## 4. 웹소켓 문제점
### &nbsp; 1) 프로그램 구현에 보다 많은 복잡성을 초래함
#### &nbsp;&nbsp; (1) 웹 소켓은 HTTP와 달리 Stateful protocol 이기 때문에 서버와 클라이언트 간의 연결을 항상 유지해야 하며 비정상적으로 연결이 끊어졌을 때 적절하게 대응해야함
#### &nbsp;&nbsp; (2) 기존의 HTTP 사용시와 비교했을 때 코딩의 복잡성을 가중시키는 요인이 될 수 있음
### &nbsp; 2) 서버와 클라이언트 간의 Socket 연결을 유지하는 것 자체가 비용 부담이 큼
#### &nbsp;&nbsp; (1) 트래픽 양이 많은 서버같은 경우 CPU에 부담이 큼
### &nbsp; 3) 오래된 버전의 웹 브라우저에서는 지원하지 않음
#### &nbsp;&nbsp; (1) SockJS 라이브러리 같은 경우 Fallback option을 제공


<br><br>


## 5. 웹소켓 사용 예
### &nbsp; 1) NS APP
### &nbsp; 2) 멀티플레이어 게임
### &nbsp; 3) 위치 기반 APP
### &nbsp; 4) 증권 거래 정보 사이트 및 APP
### &nbsp; 5) 화상 채팅 APP
### &nbsp; 6) 여러 명이 동시 접속해서 수정할 수 있는 Tool


<br><br>


## 6. 웹소켓 라이브러리
### &nbsp; 1) Socket.io
#### &nbsp;&nbsp; (1) node.js를 지원하고, websocket 기반으로 실시간 웹 애플리케이션을 위한 Javascript 라이브러리 
#### &nbsp;&nbsp; (2) 비동기 이벤트 방식을 사용해 실시간으로 간단하게 데이터를 주고받을 수 있음
#### &nbsp;&nbsp; (3) 소켓 연결 실패 시 fallback을 통해 다른 방식으로 알아서 해당 클라이언트와 연결을 시도
#### &nbsp;&nbsp; (4) 방 개념을 이용해 일부 클라이언트에게만 데이터를 전송하는 브로드캐스팅이 가능
### &nbsp; 2) SockJS
#### &nbsp;&nbsp; (1) Springframework에서 WebSocket을 기반으로 websocket과 비슷한 기능을 제공하는 javascript 라이브러리
#### &nbsp;&nbsp; (2) 스프링 메뉴얼에 webSocket 부분을 보면 브라우저 문제를 해결하기 위한 방법으로 SockJS를 솔루션으로 제공
#### &nbsp;&nbsp; (3) websocket을 지원하지 않는 브라우저도 잘 동작됨
### &nbsp; 3) STOMP
#### &nbsp;&nbsp; (1) WebSocket위에서 동작하는 메시지 프로토콜
#### &nbsp;&nbsp; (2) 클라이언트와 서버가 전송할 메시지 유형, 형식, 내용들을 정의하는 매커니즘
#### &nbsp;&nbsp; (3) Publish-Subscribe 매커니즘 제공
#### &nbsp;&nbsp; (4) Broker를 통해 다른 사용자들에게 메시지를 보내거나 서버가 특정 작업을 수행하도록 메시지를 보낼 수 있게 되는 것