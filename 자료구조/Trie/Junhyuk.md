# 트라이(Trie)

트라이는 문자열을 트리 자료구조의 일종으로 문자열의 자동 완성 기능과 같이 문자열을 저장하고 검색하는데 사용되는 자료구조, **RadicTree**, **PrefixTree**라고도 불린다.

**Trie**의 각 노드는 <Key, Value>형태의 **Map**형식을 띄고 있으며 **Key**는 하나의 알파벳이고 **Value**는 **자식 노드의 Key**가 된다

최초 접두어로부터 시작하여 단어 전체를 찾아가는 과정

**장점**

- **탐색에 매우 효율적**
- **문자열을 추가하더라도 추가와 탐색 모두 O(N)의 시간복잡도로 효율적이다**

**단점**

- **Trie는 문자열 저장을 위해 공간을 많이 쓰임**
    - 최적화를 위해 문자열 길이가 길어 Map 등을 이용해 동적 할당을 해야 하는 경우, O(MlogN)만큼의 시간복잡도 발생
    - 단, 최적화가 까다롭다.

예시

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F552fe0dc-fdb3-4c62-979e-df2a2e235613%2Fbcc67932-5bb1-48ce-bee3-c824e3782e45%2FUntitled.png?table=block&id=6f29a349-d894-46a0-a329-fafa0d794b54&spaceId=552fe0dc-fdb3-4c62-979e-df2a2e235613&width=1700&userId=a09a1ca3-4214-4905-a7a2-172e60f8cd39&cache=v2)

**구현시 참고하기 위한 링크**
[[자료구조] Trie(트라이)에 대한 개념과 활용법](https://frtt0608.tistory.com/115)