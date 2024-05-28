# 그래프

### 그래프

- 정점(vertex)과 간선(edge)로 이루어신 자료구조
- 트리 , Linked-List 도 그래프의 일종이다, 하지만 그래프는 정점마다의 간선(연결)이 있을 수도 있고 없을 수도 있다.

### 그래프 관련 용어

| 용어 | 뜻 |
| --- | --- |
| 정점(Vertex or Node) | 데이터가 저장된 위치 |
| 간선(Edge) | 정점을 연결하는 선. 링크(Link) or 브랜치(branch) 로 불림 |
| 인접 정점(adjacent vertex) | 간선에 의해 직접 연결된 정점을 의미  |
| 단순 경로(simple path) | 경로 중에서 반복되는 정점이 없는 경우를 의미 (한붓 그리기) |
| 차수(degree) | 무방향 그래프에서 하나의 정점에 인접한 정점의 수를 의미 |
| 진출 차수(in-degree) | 방향 그래프에서 외부로 향하는 간선의 수를 의미 |
| 진입 자수(out-degree) | 방향 그래프에서 자신으로 들어오는 간선의 수를 의미 |
| 경로 길이(path length) | 경로를 구성하는데 사용된 간선의 수를 의미 |
| 사이클(cycle) | 단순 경로의 시작 정점과 종료 정점이 동일한 경우를 의미 |

### 그래프의 종류

- **무방향 그래프**(Undirected Graph)
    - 두 정점을 연결하는 간선에 방향이 없는 그래프 ⇒ 간선이 양방향을 향해 있는 형식

![image](https://blog.kakaocdn.net/dn/DAqKU/btrfiwXVG8G/D1DyuXvwPQUZPcJUKakBKk/img.png)

- **방향 그래프(Directed Graph)**
    - 두 정점을 연결하는 간선에 방향이 존재하는 그래프
    - 간선의 방향으로만 이동할 수 있다
    
    ![image](https://blog.kakaocdn.net/dn/b4SbKU/btrfgYH5hXD/6p51kJ07OPgaeRS5IuEk3k/img.png)
    
- **가중치 그래프(Weighted Graph)**
    - 간선에 가중치(비용)가 할당된 그래프
    - 두 정점을 이동할 때 비용이 드는 그래프
    
    ![image](https://blog.kakaocdn.net/dn/Sq2g9/btrffQjgnNb/hMkyZqP4qR1KbKguklO41k/img.png)
    
- **연결 그래프(Connected Graph)**
    - 무방향 그래프에 있는 모든 정점 쌍에 대해서 항상 경로가 존재하는 그래프
    - 대표적으로 트리(Tree)가 있다
    
    ![image](https://blog.kakaocdn.net/dn/34w7V/btrfjcSovHh/No0igelqdwis7Qah95eMK1/img.png)
    
- **비연결 그래프(Disconnected Graph)**
    - 정점 사이에 경로가 존재하지 않는 경우가 존재하는 그래프
    - 노드들 중 간선에 의해 연결되어 있지 않은 그래프이다.
    
    ![image](https://blog.kakaocdn.net/dn/22Ijs/btrfhIEtQ5S/DZmdkCBAtxK0PX72cIXsS0/img.png)
    
- **완전 그래프(Complete graph)**
    - 그래프의 모든 정점이 서로 연결(인접 연결)되어 있는 그래프
    
    ![image](https://blog.kakaocdn.net/dn/k1G3R/btrfhIqYuso/FCkMWn7mB82yDVpsi36UK0/img.png)
    
- **순환그래프(Cycle)**
    - 단순 경로에서 시작 정점과 도착 정점이 동일한 그래프
    
    ![image](https://blog.kakaocdn.net/dn/cDwFx5/btrfjdqffI2/AKIxmvwkk9xKbEib4nrHu0/img.png)
    
- **비순환그래프(Acyclic Graph)**
    - 사이클 그래프를 제외한 그래프 사이클이 없는 그래프
    
    ![image](https://blog.kakaocdn.net/dn/bWEhuV/btrfi47VkAz/k3bxfxKDsZWOMWJBct2aC1/img.png)
    
    ### 그래프 구현 방식(위 비순환 그래프 예시)
    
    **2차원 배열을 활용한 인접 행렬 방식**
    
    |  | A | B | C | D |
    | --- | --- | --- | --- | --- |
    | A | 0 | 0 | 1 | 0 |
    | B | 1 | 0 | 0 | 0 |
    | C | 0 | 0 | 0 | 0 |
    | D | 1 | 0 | 1 | 0 |
    - 장점 :
        - 정점 간의 연결 상태를 조회할 때 인덱스를 기반으로 조회 하기 때문에 시간 복잡도가 O(n)로 낮다
    - 단점 :
        - 2차원 배열의 메모리 공간을 요구함으로 메모리 낭비가 있다(간선간 연결이 안되어도 메모리를 소비함)
        - 모든 정점의 간선 정보를 넣을 때 n^2의 시간 복잡도가 필요하다 (2중 loof)
    
    **인접 리스트 방식**
    
    **A)** ⇒ B - D
    
    **B)** ⇒ 빈 리스트
    
    **C)** ⇒ A - D
    
    **D)** ⇒ 빈 리스트
    
    - **장점 :**
        - 정점들의 연결 정보를 탐색할 때 시간 복잡도가 O(n)로 작다
        - 필요한 공간만 리스트로 만들어 사용하기 때문에 메모리 낭비가 적다
    - **단점 :**
        - 정점이 서로 연결되었는지 확인하기 위해서는 리스트를 순회해야 한다. 이는 **간선의 개수** or **정점의 수**가 많아질 수록 성능이 떨어진다