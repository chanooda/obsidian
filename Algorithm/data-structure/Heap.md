최댓값 혹은 최솟값을 빠르게 찾기 위해 완전이진트리 현태로 연산을 수행하는 자료구조
![[Heap1.webp]]
### 힙 대표 속성
- **정렬** : 각 노드의 값은 자식 노드가 가진 값보다 작거나 혹은 큰 순서대로 정렬
- **형태** : 트리의 형태는 완전이진트리 모양
### 힙 구현
완전이진트리 성질을 만족하기 때문에 1차원 배열로 표현 가능
1. 부모 노드: (N - 1) / 2
2. 현재 노드 : N                                                                           
3. 왼쪽 자식 노드: (N * 2) +1                                                            
4. 오른쪽 자식 노드: (N * 2) + 2

### 구현 메서드
1. 노드 위치 계산
2. 노드 값 확인
3. 노드 추가/삭제