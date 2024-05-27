# RED-BLACK-TREE

일반 이진트리에서 자식들이 한쪽으로 치우치는 것을 막기 위한 균형 기능이 추가된 트리. 

자가 균형 이진트리(Self-balancing binary tree)의 일종. 

**NIL node**

1. NIL node는 자식노드가 존재하지 않는 경우를 NIL node로 분류.
2. 모든 leaf node는 NIL node
3. root의 부모도 NIL node 라고 가정

노드는 **내부노드**와 **NIL node**로 나뉜다.

Red-Black-Tree 는 다음 5가지 조건을 만족하는 이진탐색트리이다.

1. 각 노드의 색은 red 또는 black이다.
2. root 노드는 black이다.
3. 모든 leaf node 는 black이다.
4. red 노드의 자식노드들은 전부 black이다. (red 노드는 연속 등장할 수 없다.)
5. Root 노드에서 시작해서 자손인 leaf노드에 이르는 모든 경우의 경로에는 동일한 개수의 black노드가 존재한다.

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2Fe9e67118-3756-4ded-9e2b-478e77393c5f%2FUntitled.png?table=block&id=9af506e8-9dea-4802-908d-92708a61922f&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=2000&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

**삽입**

[자료구조 구현 : 레드 블랙 트리 (Red Black Tree, RB tree) - 삽입](https://dev-game-standalone.tistory.com/93)

**삭제**
[자료구조 구현 : 레드 블랙 트리 (Red Black Tree, RB tree) - 삭제](https://dev-game-standalone.tistory.com/94)