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

```
TCP Header의 크기는 20 ~ 60byte다.
```

- Sourse/Destination Port Number (각 16 비트)IP 주소 + 포트 번호 = 소켓 주소라 부른다.

```
소켓은 네트워크 상에서 돌아가는 두 개의 프로그램 간 양방향 통신의 하나의 엔트 포인트다.
소켓은 포트 번호에 바인딩(연결하는 과정)되어 TCP 레이어에서 데이터가 전달되야하는 어플리케이션
을 식별할 수있게 한다.
```

- Sequence Number (32 비트)시퀸스 번호는 전송하는 데이터의 순서를 의미한다.이 시퀸스 번호를 이용해서 잘라진 Segment를 조립하는데 사용한다.이를 통해, TCP에서는 신뢰성 및 흐름제어 기능을 제공한다고 한다.
- Acknowledgement Number (확인응답 번호 / 승인 번호) (32 비트)수신하기를 기대하는 다음 바이트 번호 = (상대방이 보낸 시퀸스 번호 + 1)쉽게 말하면 숫자 200까지 배운다고 치면 100까지 배웠으니 다음에는 101부터 알려줘 이렇게 이해하면 쉽다.
- HLEN or Data offset (32 비트)32비트 워드 단위를 사용하며, 32 비트 체계에서의 1 Word = 4 bytes를 의미한다.자료에 따라 용어가 다르지만 헤더의 길이 또는 데이터의 시작 위치를 의미한다.
- Reserved (3 비트)자세히는 모르겠지만 찾아보니 차후에 사용을 위해 남겨둔 필드라고 한다.기존에는 6비트였지만 혼잡 제어 기능의 향상을 위해 3비트를 Flag 필드로 넘겨 NS, CWR, ECE 플래그가 추가했다고 한다.
- 9 개의 Flag bits (URG, ACK, PSH, RST, SYN, FIN, NS, CWR, ECE)9개의 비트 플래그이다. 이 플래그들은 현재 세그먼트의 속성을 나타낸다.

```
1. URG(Urgent Pointer 긴급 포인터)
필드에 값이 채워져있음을 알리는 플래그. 송신측 상위 계층이 긴급 데이터라고 알려주면,
긴급비트 URG를 1 로 설정하고, 순서에 상관없이 먼저 송신됨

2. ACK(Acknowledgment 승인 번호)
1로 셋팅되면, 확인번호 유효함을 뜻한다. 0으로 셋팅되면, 확인번호 미포함 (즉, 32 비트 크기의
확인응답번호 필드 무시됨)

3. PSH(Push)
수신 측에게 이 데이터를 최대한 빠르게 응용프로그램에게 전달해달라는 플래그이다. 이 플래그가 0이
라면 수신 측은 자신의 버퍼가 다 채워질 때까지 기다린다. 이 플래그가 1이라면 이후에 더 이상
연결된 세그먼트가 없음을 의미하기도 한다.

※ 아래 3개 비트 플래그(RST,SYN,FIN)는 TCP 연결설정 및 TCP 연결종료에 주도적으로 사용됨

4. RST(Reset 강제 연결 초기화)
ESTABLISHE(연결확립)된 회선에 강제 리셋 요청

5. SYN(Synchronize 연결시작, 동기화)
TCP 연결설정 초기화를 위한 순서번호의 동기화

연결요청  : SYN=1, ACK=0   (SYN 세그먼트)
연결허락  : SYN=1, ACK=1   (SYN+ACK 세그먼트)
연결설정  : ACK=1          (ACK 세그먼트)

6. FIN(Finish 종료)
상대방과 연결을 종료하고 싶다는 요청인 세그먼트임을 의미한다.

종결요청 : FIN=1           (FIN 세그먼트)
종결응답 : FIN=1, ACK=1    (FIN+ACK 세그먼트)

*NS, CWR, ECE 플래그는 네트워크의 명시적 혼잡통보(Explicit Congestio
n Notification, ECN)을 위한 플래그이다.

7. NS
CWR, ECE 필드가 실수나 악의적으로 은폐되는 경우를 방어하기 위해 추가된 필드

8. ECE(ECN Echo)
해당 필드가 1이면서, SYN 플래그가 1일 때는 ECN을 사용한다고 상대방에게 알리는 의미.
SYN 플래그가 0이라면 네트워크가 혼잡하니 세그먼트 윈도우(데이터의 양)의 크기를 줄여달라는
요청의 의미이다.

9. CWR
이미 ECE 플래그를 받아서, 전송하는 세그먼트 윈도우의 크기를 줄였다는 의미이다.
```

- Window Size (16 비트)윈도우 사이즈 필드에는 한번에 전송할 수 있는 데이터의 양을 의미하는 값을 담는다. 16 비트이기 때문에 2^16 = 65535 이라는 범위를 가진다.
- Checksum (16 비트)체크섬은 데이터를 송신하는 중에 발생할 수 있는 오류를 검출하기 위한 값이다.

```
16비트는 너무 길어서 8비트로 예를 들면
1의 보수(쉽게 주어진 값이 1이면 0인 반대값으로 생각)를 취하고, 그 합에 대한 결과를 전송하면
수신측에서, 같은 합을 해보아서 오류를 검출하는 방식이다.

  10001010
+ 01110101
-----------
  11111111

검사합의 값이 0 이면 오류 없음, 0 이 아니면 오류 있음
```

- Urgent Pointer (16 비트)TCP 세그먼트에 포함된 긴급 데이터의 마지막 바이트에 대한 일련번호
- Options (0 ~ 40 바이트)TCP 연결 관리 기능을 확장시키는데 주로 사용되는 옵션

**UDP header**

1. 소스 포트 번호 (2 바이트)
2. 대상 포트 번호 (2 바이트)
3. 데이터 길이 (2 바이트)
4. UDP 체크섬 (2 바이트)

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F013f0b65-af5c-41b2-9a4f-4cf98dfe7eba%2FUntitled.png?table=block&id=c073f0cf-0763-4fb7-901b-6246c11e1a42&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

UDP 헤더는 TCP 헤더와 비교하면 상당히 간단하다.

목적지와 출발지 간단한 조건만 맞으면 전송해서 신뢰성이 없다라고 알아두자.

```
IP Header의 크기는 IPv4일 경우 20 ~ 40 바이트고 IPv6일 경우 40 바이트 고정이다.
```

- Version (4 비트)IPv4, IPv6인지 구별하기 위해 사용한다.
- Header Length (4 비트)헤더의 길이를 32 비트(4 바이트) 워드 단위로 표시한다. 최소 5 ~ 15까지의 값을 가진다.(5 * 32 = 160 비트 or 20 바이트)
- Type of Service (8 비트)
    
    요구되는 서비스 품질을 나타낸다.
    
- Total Packet Length (16 비트)
    
    IP 헤더 및 데이터를 포함한 Packet 전체의 길이를 바이트 단위로 길이를 표시한다.
    
    최대값은 2^16 - 1 = 65,535
    
- Fragmentation Identifier (16 비트)분열이 발생한 경우, 각 조각이 동일한 데이터그램에 속하면 같은 일련번호를 공유한다.
- Fragmentation Flag (3 비트)분열의 특성을 나타내는 플래그이다.

```
첫번 째 bit : 미사용 (항상 0)
두번 째 bit : D F bit (Don't Fragment)
세번 째 bit : M F bit (More Fragment)

0 = NO , 1 = OK 
```

- Fragmentation Offset (13 비트)8 바이트 단위(2 워드)로 최초 분열 조각으로부터 어떤 곳에 붙여야하는 위치를 나타낸다.
- Time to live (8 비트)TTL은 1 ~ 255사이의 값을 지정하며 Router는 패킷을 전달 할 때마다 이 값을 하나씩 감소시킨다. <- 루프(패킷이 목적지에 도착하지않고 계속 돌아다니는 현상)를 방지하기 위해 생겨났다.
- Protocol Identifier (8 비트)번역한 그대로 프로토콜 식별자이다 어느 상위계층 프로토콜이 데이터 내에 포함되었는가를 보여준다.

```
ICMP = 1
IGMP = 2
TCP  = 6
EGRP = 8
UDP  = 17
OSPF = 89
...
```

- Header Checksum (16 비트)헤더에 대한 오류검출
- Source IP Address (32 비트)출발지 IP 주소를 의미한다.
- Destination IP Address (32 비트)목적지 IP 주소를 의미한다.
- Option (선택적 필드)
- Padding (필요한 경우 사용)기본 블록 크기/길이를 맞추기 위한처리만약, 마지막 블록이 블록 길이에 미치치 못할 경우 적당한 비트열을 채워 넣는다.