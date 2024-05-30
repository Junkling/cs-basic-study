# 소켓(Socket)이란

## 웹 소켓의 등장

초기의 인터넷 통신 방식은 HTTP를 이용한 클라이언트(요청) - 서버(응답) 모델을 통해 진행되었다.
이는 클라이언트가 서버의 요청(Request)하나에 서버가 응답(Response)하는 통신 방식이였다는 것이다.
 
이 방식이 페이지를 단혈적인 요청에는 효과적이지만 실시간으로 데이터를 주고받아야 할 때는 불리하다.

기존 http 요청의 방식으로는 실시간 데이터를 주고받는 환경을 구현하기 위해 클라이언트는 항상 새로운 데이터가 있는지 확인을 하기 위해 서버에 지속적으로 요청을 보내야한다.
 
이는 요청과 응답을 증가시킴으로 트래픽을 불필요하게 증가시키고, 이로 인해 서버의 비용이 증가될 뿐더러 요청과 응답사이의 지연시간이 있기 때문에 실시간 통신의 효율성을 저하시킨다.

## 소켓 통신

웹 소켓은 HTML5에 등장 실시간 웹 애플리케이션을 위해 설계된 통신 프로토콜이며, TCP를 기반으로 한다.
 
HTTP와 다르게 클라이언트와 서버 간에 최초 연결이 이루어지면, 이 연결을 통해 양방향 통신을 지속적으로 할 수 있다. 
**지속적 연결**을 통해서 서버도 클라이언트에 실시간으로 데이터를 보낼 수 있게 되고, 반대로 클라이언트도 서버에게 데이터를 보낼 수 있다.

## 소켓의 동작 방식

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F90c09c3f-cc8f-4de6-a917-c2ed6842caab%2FUntitled.png?table=block&id=a4f74c8c-89c9-4534-bcbf-46f0823c0b08&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=1700&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

### Server

1. **socket**: 소켓 생성(TCP는 stream)
    - 새로운 client의 요청을 대기하기 위함
2. **bind**: 사용할 IP address와 Port number 등록
    - 운영체제에서 소켓이 중복된 포트 번호를 사용하지 않도록, 내부적으로 포트 번호와 소켓 연결 정보를 관리한다.
3. **listen**: 연결 되지 않은 소켓을 요청 수신 대기 모드로 전환(Block 상태)
4. **accept**: client의 요청dmf 수락 → 통신을 위한 새로운 소켓 생성(실질적인 소켓 연결)

### Client

1. **socket**: 마찬가지로 소켓 생성(TCP는 stream)
2. **connect**: Client에서 Server와 연결하기 위해 소켓과 목적지 IP address, Port number 지정 (Block 상태)

### Server-Client

- **send**, **recv**: Client는 처음에 생성한 소켓으로, Server는 새로 반환(생성)된 소켓으로 client와 server간에 데이터 송수신
- **close**: 소켓을 닫음

## 엔드 포인트란?

엔드 포인트는 아이피 주소와 포트 번호의 조합을 의미. 클라이언트와 서버의 엔트포인트를 기반으로 TCP 3way-handshack를 진행하며 여러 클라이언트가 서버의 엔드포인트에 연결을 맺을 수 있다.