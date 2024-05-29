# B-Tree & B+Tree & B*Tree

모두 균형 이진트리의 일종

**이진 트리 :** 각 노드가 최대 두개의 자식노드를 가지는 트리 자료구조

**균형 이진 트리 :** 이진 트리가 한쪽으로 치우치지 않도록 균형을 유지하는 트리 구조

- **종류 :** AVL 트리 , RED-BLACK 트리 , B-Tree, B+Tree, B*Tree 등

**BST의 시간 복잡도**

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2Fd2cf6218-30cd-4afd-9dfe-f9d872cc9cd5%2FUntitled.png?table=block&id=b3deb1a1-f605-4837-8666-35c827430b27&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=1710&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

**만약 편향 트리의 모습이 된다면??** ⇒ Wosrt case O(n)

- 높이가 높아질수록 성능이 떨어진다.

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2F1b99cc6d-b9fd-4cb5-bdae-6e0cd60bb48c%2FUntitled.png?table=block&id=b48a6c1d-3431-4050-a1cc-7c37a6f33cac&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=1710&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

# B tree

B tree는 하나의 노드에 2개의 자식이 아닌 여러 자식이 배치되는 구조이다.

한 노드에 M개의 자료가 배치되면 M차 B-Tree라고 한다.

생성 시 자식 노드의 개수가 M개 이상이면 높이를 높이는 재구조화

트리의 높이를 낮출 수 있

**B-Tree 실행할 수 있는 사이트**

[B-Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

# B* Tree

B-Tree는 적은 수의 노드를 생성하는 것이 인덱스 구조의 성능을 높일 수 있다. 때문에 생성되는 노드의 수를 줄이기 위해 B-Tree의 변형으로 B* 트리가 나옴

B-tree는 삽입 과정에서 **분열과 삭제** ,**합병** 등의 **보조 연산**이 필요하다.

B* Tree에서는 이런 보조 연산을 가급적 **지연**시켜 회수를 감소시키 준.

### **특징**

- 노드의 분열 감소
    - 노드가 가득차면 분열하는 대신 가까운 형제 노드로 재배치 한다.
    - 인접 노드까지 모두 가득찰 때까지 분열을 지연한다.
    - 인접 노드에도 overflow가 일어나서 더 이상 빈 자리가 없을 경우, 가득찬 두 노드를 분열하여 2/3 정도 채워진 3개의 노드로 만든다.
        - 재배치 동작은 2번 발생한다.
- 모든 노드는 2/3 이상 채워져야 된다. (Root 노드 제외)
    - B-tree는 1/2 이상
- 삭제 시 key 값의 개수가 모자라면 이웃한 형제 노드로부터 재배치하고 재배치도 할 수 없는 경우 합병한다.
    - 세 개의 노드를 두 개의 노드로 합병한다.

# B+ tree

B-tree의 복잡한 순회를 해결하기 위한 B+ tree

인덱스 구조에서 순차 접근에 대한 문제의 해결책으로 제시되었다.

B-트리에서는 **특정 key 값이 하나의 노드에서만 존재**할 수 있지만, B+ tree에서는 **leaf 노드와 leaf의 부모 노드에서 공존**할 수 있다.

### **특징**

- index 부분과 leaf 노드로 구성된 순차 data 부분으로 이루어진다.
- Index 부분의 key 값은 leaf에 있는 key 값을 직접 찾아 가는데 사용하고 모든 key 값은 leaf 노드에 나열된다.
    - index 부분의 key 값도 leaf 노드에 다시 한 번 나열된다.
- Leaf 노드는 순차적으로 linked list를 구성하고 있어서 순차적 처리가 가능하다.
- 노드의 분열이 일어나면 중간 key 값이 부모 노드로 올라간다.
    - 이는 새로 분열된 노드에도 포함되어야 한다.
- 새 노드는 leaf 노드끼리의 linked list에도 삽입되어야 한다.
- 재배치와 합병이 필요하지 않을 때는 leaf 노드에서만 삭제된다.
- leaf node의 값이 삭제되어도 삭제하지 않는다.
    - 다른 key 값을 찾는데 사용될 수 있기 때문
- 재배치할 경우 노드의 key 값은 변하지만 tree 구조는 변하지 않는다.
- 합병을 할 경우 index 부분에서도 key 값을 삭제한다.

# B tree & B+ tree

![image](https://blog.kakaocdn.net/dn/ects5q/btr0okrAv1u/TjPsQfsESmeVxJTAJLKcj0/img.png)

### **공통점**

- 모든 leaf의 깊이(depth)는 같다.
- 각 node에는 k/2 ~ k 개의 item이 들어있다.
- 검색(순회)가 비슷하다.
- 삽입 시 overflow가 발생하면 분열한다.
- 삭제 시 underflow가 발생하면 재배치하거나 합병한다.

### **차이점**

### **노드의 Key와 데이터**

- B-tree: 각 노드에 key값 + 데이터도 들어간다.
    - 데이터는 디스크 블럭으로부터 포인터가 될 수 있다.
- B+ tree: 각 노드에 key값만 들어가야 한다.
    - 데이터는 오직 leaf에만 존재한다.

### **add, delete**

- B+ tree: add, delete가 leaf에서만 이루어진다.

### **linked list**

- B+ tree: leaf 노드끼리 linked list로 연결되어 있다.
    - 연속적인 범위 탐색에 매우 유리하다.