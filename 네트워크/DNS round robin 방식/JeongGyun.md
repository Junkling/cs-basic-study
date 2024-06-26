# 네트워크 - DNS round robin 방식
> DNS 라운드 로빈 방식은 네트워크 부하 분산을 위해 사용하는 기술임

라운드 로빈 방식
-  모든 순서가 차례로 계속되고 후에 다시 첫번 째 것이 기회를 갖게됨
    - 분류되어진 여러 큐에다가 각각 보낼 수 있는 기회를 차례로 주는 방식
      . 즉, 모든 큐가 공정하게 기회를 갖음

       일명, `수건 돌리기`라고 함

-  스케줄링 기법 중 하나

![스크린샷 2024-05-25 오전 12 22 55](https://github.com/5dotseven/cs-basic-study/assets/144773042/d3d1cad3-1614-4a28-a695-b534cb9117bd)

이 방식은 여러 IP 주소를 하나의 도메인 이름에 매핑하여, 각 요청에 대해 순차적으로 다른 IP 주소를 반환함
예를 들어, example.com 도메인에 A 레코드를 통해 3개의 IP 주소(192.0.2.1, 192.0.2.2, 192.0.2.3)를 설정했다고 가정하면, 첫 번째 요청에는 192.0.2.1을 반환하고, 두 번째 요청에는 192.0.2.2를, 세 번째 요청에는 192.0.2.3을 반환하는 식임.
이렇게 함으로써 트래픽을 여러 서버에 분산시켜, 특정 서버에 과부하가 걸리지 않도록 조절할 수 있음.

DNS 라운드 로빈 방식은 설정이 간단하고 특별한 하드웨어나 소프트웨어를 필요로 하지 않기 때문에 많이 사용됨

그러나 이 방식은 각 서버의 상태를 모니터링하지 않기 때문에, 만약 특정 서버가 다운되었을 때도 그 서버로 트래픽이 전달될 수 있다는 단점이 있음 이를 보완하기 위해 헬스 체크 기능을 추가하거나, 더 복잡한 로드 밸런싱 솔루션과 함께 사용하는 것이 좋음.

참고: http://www.ktword.co.kr/test/view/view.php?m_temp1=2491&id=1069