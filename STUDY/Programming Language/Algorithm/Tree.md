노드/원소 의 앞-뒤 관계가 1 : 1 인 구조인 선형 자료 구조와는 다르게 노드의 앞-뒤 관계가 1 : n 또는 n :  n 이다.

다수의 노드가 연결된 구조를 Tree 라 한다. (하나의 노드가 다수의 노드를 참조)

즉 비선형적이고 계측적인 자료구조

### 구조

- Root : 최상위 계층의 단 하나 노드

- Edge : 노드와 노드 사이를 연결해주는 것

- Parent - Child  : Edge 로 연결된 두 노드에 대해서 서로 인식하는 기준

-  Leaf  : 가장 하단의 노드

- SubTree : Tree 중 하나의 노드를 중심으로 그 자식 노드 일부를 포함한 부분

- Sibling : 같은 부모를 가지고 있는 노드 사이

- Size : Root 를 포함한 모든 Node 의 수

 - Depth : 특정 노드에서 root 까지의 거리. Sibling 관계의 노드 끼리는 동일한 Depth 이다. Root 노드의 경우 Depth 가 0 이다.

- Height : 노드 Depth 중 최대 값

## 정의

- 각 Node 는 하나의 객체로 표현된다.
- Node 에는 Data 와 Child Node 에 대한 참조 정보를 갖는다.

## 종류

- Binary Tree  : 각 노드가  최대 2개의 child 노드를 갖는다.\

- Binary Search Tree : 다음의 조건을 갖는다.
	1. 왼쪽 자식 노드 값 < 부모 노드 값
	2. 부모 노드 값 < 오른쪽 자식 노드 값

## 기능

- Tree Traversal (트리 순회)
	- (DFS , 깊이 우선 탐색 을 사용해 순회)
	1. In-Order Traversal (중위 순회) :  (Left -> visit -> right)  (d - b - e - a - f - c - g)
	2. Pre_Order Traversal (전위 순회)  : (visit -> Left -> right) (a - b - d - e - c - f - g)
	3. Post_Order Traversal (후위 순회) : (left -> right -> visit) (d - e - b - f - g - c - a)
	![[Pasted image 20240610144911.png]]