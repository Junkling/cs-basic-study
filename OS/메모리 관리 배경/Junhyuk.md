# 메모리 관리 배경

### 단편화 (Fragmentation)

컨텍스트 스위칭 과정 중 발생하는 현상으로,

메모리 공간이 남아 있지만 프로세스가 메모리에 적재되지 못해 메모리가 낭비되는 현상.

### 1) 외부 단편화 (External Fragmentation)

가변 분할 방식에서 메모리에 프로세스가 적재되고 제거되는 일이 반복되면서 여유 공간들의 조각으로 흩어져 있어 (Scarttered Holes) 메모리에 프로세스를 적재하지 못해 메모리가 낭비되는 현상

### 2) 내부 단편화 (Internal Fragmentation)

고정 분할 방식에서 프로세스가 실제 사용할 메모리보다 더 큰 메모리를 할당 받아서 메모리가 낭비되는 현상
