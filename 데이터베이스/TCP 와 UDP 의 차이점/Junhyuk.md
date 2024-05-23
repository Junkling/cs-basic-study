# TCP/IP의 개념

### **TCP란?**

최상위 계층인 TCP는 많은 양의 데이터를 가져 와서 패킷으로 컴파일 한 다음 동료 TCP 계층에서 수신하도록 전송하여 패킷을 유용한 정보 / 데이터로 바꾸는 역할을 함. TCP는 전달받은 패킷을 재조립하고, 패킷에 손상이 있거나 손실된 패킷이 있다면 재전송을 요청하는 패킷을 전송하여 재전송 받는다.

### **IP란?**

IP는 Internet Protocol의 줄임말로, 인터넷에서 컴퓨터의 위치를 찾아서 데이터를 전송하기 위해 지켜야 할 규약. 전 세계 수억대의 컴퓨터가 인터넷을 하기 위해서는 서로의 정체를 알 수 있도록 특별한 주소를 부여했는데 이 주소를 IP주소라고 함.

### TCP/IP 4계층

1. 네트워크엑세스 계층(물리계층, 데이터링크계층)
2. 인터넷계층(네트워크계층),
3. 전송계층(전송계층)
4. 응용계층(세션계층, 표현계층, 응용계층)

### **네트워크 액세스 계층(Network Access Layer)**

1. OSI 7계층의 물리계층과 데이터 링크 계층에 해당
2. TCP/IP 패킷을 네트워크 매체로 전달하는 것과 네트워크 매체에서 TCP/IP 패킷을 받아들이는 과정을 담당
3. 에러 검출 기능(Detecting errors), 패킷의 프레임화(Fraimg packets)
4. 네트워크 접근 방법, 프레임 포맷, 매체에 대해 독립적으로 동작하도록 설계.
5. 물리적인 주소로 MAC을 사용
6. LAN, 패킷망, 등에 사용됨

### **2계층 인터넷 계층(Internet Layer)**

1. OSI 7계층의 네트워크 계층에 해당
2. 어드레싱(addressing), 패키징(packaging), 라우팅(routing) 기능을 제공
3. 네트워크상 최종 목적지까지 정확하게 연결되도록 연결성을 제공하게 됨.
4. 프로토콜 종류 – IP, ARP, RARP

### **3계층 전송 계층(Transport Layer)**

1. OSI 7계층의 전송 계층에 해당
2. 애플리케이션 계층의 세션과 데이터그램(datagram) 통신서비스 제공
3. 통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터 전송을 담당한다.
4. 프로토콜 종류 – TCP, UDP

[[Network] TCP / UDP의 개념과 특징, 차이점](https://coding-factory.tistory.com/614)

### **4계층 응용 계층(Application Layer)**

1. OSI 7계층의 세션 계층, 표현 계층, 응용 계층에 해당한다.
2. 프로그램(브라우저)가 직접 인터액트하는 레이어. 데이터를 처음으로 받는곳
3. 다른 계층의 서비스에 접근할 수 있게 하는 애플리케이션을 제공
4. 애플리케이션들이 데이터를 교환하기 위해 사용하는 프로토콜을 정의
5. HTTP, SMTP등의 프로토콜을 가진다.
6. TCP/UDP 기반의 응용 프로그램을 구현할 때 사용한다.
7. 프로토콜 종류 – FTP, HTTP, SSH

**TCP Header 구성**

1. 출발지 포트 (2)
2. 도착지 포트 (2)
3. 시퀀스 넘버 (4)
4. Acknowledgement(응답) 넘버 (4)
5. 플래그 (2) [tcp header length(4bit), reserved(6bit), control flags(6bit)]
6. 윈도우 사이즈 (2)
7. 체크섭(2)
8. urgent pointer (2)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/013f0b65-af5c-41b2-9a4f-4cf98dfe7eba/Untitled.png)

**UDP header**

1. 소스 포트 번호 (2 바이트)
2. 대상 포트 번호 (2 바이트)
3. 데이터 길이 (2 바이트)
4. UDP 체크섬 (2 바이트)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b8427d5-fd75-43a2-a1bb-5acb73cd7201/Untitled.png)
