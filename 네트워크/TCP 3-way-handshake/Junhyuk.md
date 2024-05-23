# TCP의 3-way-handshake와 4-way-handshake
- Q. TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?
- Q. 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?
- Q. 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?

**TCP 3 Way Handshake는 TCP/IP프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에**

**먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다..**

Client > Server : TCP SYN

Server > Client : TCP SYN ACK

Client > Server : TCP ACK

- **TCP의 3-way Handshaking 과정**

**[STEP 1]**

A클라이언트는 B서버에 접속을 요청하는 SYN 패킷을 보낸다. 이때 A클라이언트는 SYN 을 보내고 SYN/ACK 응답을 기다리는SYN_SENT 상태가 되는 것이다.

**[STEP 2]**

B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다. 이때 B서버는 SYN_RECEIVED 상태가 된다.

**[STEP 3]**

A클라이언트는 B서버에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 되는것이다. 이때의 B서버 상태가 ESTABLISHED 이다.

위와 같은 방식으로 통신하는것이 신뢰성 있는 연결을 맺어 준다는 TCP의 3 Way handshake 방식이다.

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ffbb1feaa-e19d-4374-af01-bf232eee5995%2FUntitled.png?table=block&id=1d3043d4-fa17-4333-ad53-68aa7471dcf5&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

- **TCP의 4-way Handshaking 과정**

**[STEP 1]**

클라이언트가 연결을 종료하겠다는 FIN플래그를 전송한다.

**[STEP 2]**

서버는 일단 확인메시지를 보내고 자신의 통신이 끝날때까지 기다리는데 이 상태가 **TIME_WAIT**상태다.

**[STEP 3]**

서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN플래그를 전송한다.

**[STEP 4]**

클라이언트는 확인했다는 메시지를 보낸다.

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc6ab59ad-0985-407a-b1c3-a23fff1d81fe%2FUntitled.png?table=block&id=aa49f162-0b0a-4270-a8fa-2801e51ee967&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)
