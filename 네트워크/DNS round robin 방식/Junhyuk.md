# DNS

- **웹사이트에 접속 할 때 우리는 외우기 어려운 IP 주소 대신 도메인 이름 사용.**
- **입력한 도메인을 실제 네트워크상에서 사용하는 IP 주소로 바꾸고 해당 IP 주소로 접속하도록 하는 시스템**
- **이러한 시스템은 전세계적으로 약속된 규칙을 공유한다.**
- **상위 기관에서 인증된 기관에게 도메인을 생성하거나 IP 주소로 변경할 수 있는 ‘권한’을 부여한다.**
- **DNS는 이처럼 상위 기관과 하위 기관과 같은 ‘계층 구조’를 가지는 분산 데이터베이스 구조를 가진다**

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F64bce5f4-0803-4b5f-913b-088dc587f93e%2FUntitled.png?table=block&id=96de2061-30ea-4ca4-b4f6-7d1576f7d9c3&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

### **DNS 캐싱**

DNS 지역 성능 향상과 네트워크의 DNS 메세지 수를 줄이기 위해서 캐싱을 사용한다.

- DNS 서버가 DNS 응답을 받았을 때 그것을 로컬 메모리에 응답 정보를 저장할 수 있다.
- 호스트 이름과 IP 주소 쌍이 DNS 서버에 저장되면 처음 브라우저가 캐싱된 DNS를 확인하고 캐싱된 기록이 없을 때 DNS 질의로 넘어간다.
- 호스트 이름과 IP 주소 사이 매핑과 호스트는 영구적이지 않기 때문에 특정 기간마다 저장된 정보를 제거한다.

## **DNS Round Robin**

> round robin이란 DNS 서버 구성 방식 중 하나다.
> 

![image](https://blog.kakaocdn.net/dn/kIrwn/btrc8hoWQLp/Odb8ETK7pruJ5te9eayL20/img.png)

### **원리**

- 웹 서비스를 담당할 여러 대의 웹 서버는 자신의 공인 IP를 각각 가지고 있다.
- 사이트 접속을 위해 사용자가 해당 도메인 주소를 브라우저에 입력하면 DNS는 도메인의 정보를 조회하는데 이때 IP주소를 여러 대의 서버 IP리스트 중에서 라운드 로빈 형태로 랜덤하게 하나 혹은 여러개를 선택하여 사용자에게 알려준다.
- 결과적으로 웹 사이트에 접속하는 다수의 사용자는 실제로는 복수의 웹 서버에 나뉘어 접속하도 되면서 자연스럽게 서버의 부하가 분산되는 방식이다.

> 라운드 로빈 DNS는 여러개의 IP주소를 결과로 돌려준다.
> 
- 

### **단점**

**1. 서버의 수 만큼 공인 IP 주소가 필요합니다.**

부하 분산을 위해 서버의 대수를 늘리기 위해서는 그 만큼의 공인 IP 가 필요함.

**2. 균등하게 분산되지 않다.**

모바일 사이트 등에서 문제가 될 수 있는데, 스마트폰의 접속은 캐리어 게이트웨이 라고 하는 프록시 서버를 경유 한다.

프록시 서버에서는 이름변환 결과가 일정 시간 동안 캐싱되므로 같은 프록시 서버를 경유 하는 접속은 항상 같은 서버로 접속한다.

또한 PC 용 웹 브라우저도 DNS 질의 결과를 캐싱하기 때문에 균등하게 부하분산 되지 않다.

DNS 레코드의 TTL 값을 짧게 설정함으로써 어느 정도 해소가 되지만, TTL 에 따라 캐시를 해제하는 것은 아니므로 반드시 주의가 필다.

**3. 서버가 다운되도 확인이 불가능함.**

DNS 서버는 웹 서버의 부하나 접속 수 등의 상황에 따라 질의결과를 제어할 수 없다.

웹 서버의 부하가 높아서 응답이 느려지거나 접속수가 꽉 차서 접속을 처리할 수 없는 상황인 지를 전혀 감지할 수가 없기 때문에 어떤 원인으로 다운되더라도 이를 검출하지 못하고 유저들에게 제공된다.

이때문에 유저들은 간혹 다운된 서버로 연결이 되기도 하죠.

DNS 라운드 로빈은 어디까지나 부하분산 을 위한 방법이지 다중화 방법은 아니므로 다른 S/W 와 조합해서 관리할 필요가 있다.

이러한 방식을 라운드로빈 DNS (Round-Robin DNS)라고 한다. 일반적인 라운드로빈 방식과 다르게 별도의 LB장비나 소프트웨어 방식을 사용하지 않고, DNS에서 레코드 정보를 조회하는 시점에서 트래픽을 분산한다.

웹에서 사용하는 HTTP/HTTPS에서만 사용하는 것이 아니라 FTP, SMTP 등에서도 사용할 수 있다. 

서버는 각각의 공인 IP를 가지고 있고 사용자가 도메인을 통해 접속하는 순간 DNS에서 레코드 조회시 반환하는 하나의 공인 IP로 접속하게 된다. 

이러한 방식을 통해 자연스럽게 각 서버에 부하가 분배된다.

라운드로빈 DNS를 통해 반환된 공인 IP 리스트를 처리하는 표준 절차가 정해져 있지 않기 때문에 일반적으로 DNS 쿼리 요청시 첫 번째로 도착한 주소로 연결을 시도하거나, 애플리케이션이 임의로 선택하는 경우도 존재하며, A 레코드간의 순환이 이루어질 수 있다. 

하지만 라운드로빈 DNS는 LB 처럼 health check 기능이 없기 때문에 목록에 있는 주소 중 하나의 서비스가 실패하면 DNS는 해당 주소를 계속 제공하고 클라이언트는 여전히 작동 불가능한 서비스에 도달하려고 시도한다. 

이는 서비스 장애로 나타날 수 있으며, 네트워크 응답시간이나 서버 부하상태와 같은 상태를 고려하지 않는다. 따라서 무중단 서비스에서 라운드로빈 DNS를 사용하면 안된다.

이러한 문제점을 해결하기 위해 **GLSB (Global server load balancing)**를 사용할 수 있다. 

GLSB는 등록된 호스트에게 주기적으로 health check를 수행하고, 네트워크의 거리 혹은 지역에 따라 주기적으로 성능을 측정하고 결과를 저장하여 DNS 쿼리시 지리적, 네트워크 거리가 가까운 서버를 반환한다. 물론 지리적으로 가까운 서버는 RTT도 작기 때문에 동일한 결과를 반환하는 경우도 많다.