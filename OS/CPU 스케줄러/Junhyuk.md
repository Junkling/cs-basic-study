# CPU 스케줄러

### 시분할 시스템

- 여러 명의 사용자가 사용하는 시스템에서 컴퓨터가 사용자들의 프로그램을 번갈아가며 처리함으로 여러 사용자가 독립 컴퓨터를 사용하는 느낌을 준다.
- 예를 들어 여러 프로세스를 실행중이라면 각 프로세스들은 프로세서(CPU)에 특정 시간을 배당 받는다. 이를 시분할 시스템이라고 한다.**(Context-Switching)**
- CPU는 초당 매우 빠르게 처리가 가능하기에 여러 프로세스가 동시에 처리되는 느낌을 받는다.
- 시분할은 스케줄링으로 관리되며 스케줄링에는 아래와 같은 종류가 있다.
    - **FIFO (First In First Out)** :
        - 선입 선출
        - 대화식 시스템에 부적합
    - **RR (Round Robin) 스케줄링** :
        - FIFO 스케줄링에 우선순위를 부여하여 스케줄링 기법
        - 프로세스는 FIFO 형태로 큐에 적재되지만 주어진 시간안에 작업을 마쳐야하며 시간이 다 소비되고도 작업이 끝나지 않으면 다시 맨뒤로 간다.
    - 등등
    
    ### 스케줄링 알고리즘 종류
    
    ### FCFS(First Come First Served)
    
    - 비선점형 스케줄링 방식
    - 먼저 도착한 프로세스가 CPU를 먼저 할당
    - 콘보이 현상 발생 가능
    
    ! 콘보이 현상(Convoy Effect)
    
    - burst time 이 긴 프로세스가 먼저 도착해 다른 프로세스의 실행 시간이 전부 늦춰져 효율을 떨어뜨리는 현상
    
    ### SJF(Shortest Job First)
    
    - 비선점형 스케줄링 방식
    - burst time이 짧은 프로세스가 먼저 CPU를 할당
    - Starvation 발생 가능
    
    ! 기아 현상(Starvation)
    
    - 계속해서 우선순위가 높은 프로세스(burst time이 짧은)가 먼저 실행되어 먼저 도착했어도 우선순위가 낮은 프로세스(burst time이 긴)가 게속해서 CPU를 할당받지 못하는 현상
    
    ### SRT(Shortest Remaining Time First)
    
    - 선점형 스케줄링 방식
    - 남은 burst time이 더 짧은 프로세스에 CPU를 할당
    - Starvation 발생 가능
    1. 우선순위 스케줄링(Priority Scheduling)
    - 선점과 비선점 두가지 방식에 모두 적용 가능
    - 우선순위가 높은 프로세스에 CPU를 먼저 할당
    - 기아 현상과 무기한 봉쇄가 발생할 수 있으며 에이징 기법을 통해 해결
    
    ! 에이징 기법(Aging)
    
    - 먼저 도착한 프로세스가 나이를 계속 먹으며 우선순위가 올라가는 기법
    
    ### 라운드 로빈(RR, Round Robin)
    
    - 선점형 스케줄링 방식
    - 프로세스는 FIFO 형태로 큐에 적재되지고 할당 시간(Time Quantum)동안 작업을 실행한다 시간이 다 소비되고도 작업이 끝나지 않으면 다시 맨뒤로 간다.
    - 응답시간이 빠르며, 모든 프로세스가 공정하게 CPU를 할당받을 수 있음을 보장
    - 단, CPU 할당 시간(Time Quantum)이 길 경우, FCFS랑 같아짐
    
    ### 다단계 큐(Multilevel Queue)
    
    - 선점 방식
    - Background에서 돌아가는 프로세스와 Foreground의 프로세스에 다른 알고리즘을 적용하는 방식
    - 큐 사이에 서로 다른 CPU 할당 시간(Time Quantum)을 적용
    - Backgounrd 프로세스에는 FCFS, Foreground는 RR 알고리즘 적용
    
    ### 다단계 피드백 큐(Multilevel Feedback Queue)
    
    - 프로세스가 큐 사이를 이동 가능
    - 각 큐에 서로 다른 CPU 할당 시간(Time Quantume)을 적용함으로써, 프로세스가 해당 시간 동안 작업을 다 처리하지 못했다면, 점점 긴 Time Quantum을 할당해주는 큐로 이동
    - 우선순위는 Time Quantum이 짧은 큐가 높은
    - Starvation이 발생 가능하며 Aging으로 해결 가능
