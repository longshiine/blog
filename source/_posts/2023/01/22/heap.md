---
title: <파이썬 알고리즘 인터뷰> - 15장. 힙
categories:
  - Tech
  - 알고리즘
tags: [Heap]
date: 2023-01-22 20:50:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a>

{% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 15장: 힙

> 힙은 힙의 특성(최소 힙에서는 부모가 항상 자식보다 작거나 같다)을 만족하는 거의 완전한 트리인 특수한 트리 기반의 자료구조다.

- **힙은 그래프나 트리와는 전혀 관계 없어 보이는 독특한 이름과 달리, 트리 기반의 자료구조다**

  - 우선순위 큐를 사용할때 매번 활용했던 heapq 모듈이 힙으로 구현되어있다.
    - 파이썬에서는 최소 힙만 구현되어 있다.
  - 최소 힙은 부모가 항상 자식보다 작기 때문에 루트가 결국 가장 작은 값을 갖게 되며, 우선순위 큐에서 가장 작은 값을 추출하는 것은 매번 힙의 루트를 가져오는 형태로 구현된다.
  - **우선순위 큐는 주로 힙으로 구현하고, 힙은 주로 배열로 구현한다.**
    → 그러므로 우선순위큐는 배열로 구현되어있다.

- **힙은 정렬되어 있는 구조가 아니다 → 부모 노드가 자식 노드 보다 작다는 조건만 만족!**

  - 부모, 자식 간의 관계만 정의, 좌우에 대한 관계는 정의 X
  - 자식이 둘인 힙은 이진 힙이라 하며, 대부분은 이진 힙이 널리 사용된다.

- **힙은 항상 균형을 유지하는 특징 때문에 다양한 분야에 널리 활용된다.**

  - 우선순위 큐 뿐만아니라 이를 활용한 다익스트라 알고리즘에도 활용된다.

- **BST는 좌/우 관계를 보장한다. 힙은 상/하 관계를 보장한다.**
  - BST는 탐색과 삽입 모두 O(log n)에 가능 → 모든 값이 정렬되어야 할때 사용
  - 가장 큰 값을 추출하거나(최대 힙), 가장 작은 값을 추출할 때 (최소 힙) → 이진 힙 사용
    - 이진 힙은 이 작업이 O(1)에 가능하다
    - 우선순위와 관련되어 있으며, 이진 힙은 우선순위 큐에 사용된다.

### 힙 연산

---

- 힙 선언 → heapq 모듈에서 지원하는 최소 힙 연산 → 리스트로 구현
  ```python
  class BinaryHeap(object):
  	def __init__(self):
  		self.items = [None]
  	def __len__(self):
  		return len(self.items) - 1
  ```
- 삽입(Up-Heap) → percolate_up() = **heapq.heappush**

  - **시간 복잡도 = O(log n)**

  1. 요소를 가장 하위 레벨의 최대한 왼쪽으로 삽입한다. (배열로 표현할 경우 가장 마지막에 삽입)
  2. 부모 값과 비교해 값이 더 작은 경우 위치를 변경한다
  3. 계속해서 부모값과 비교해 위치를 변경한다.

  ```python
  def _percolate_up(self):
  	i = len(self.items)
  	parent = i // 2
  	while parent > 0:
  		# 값이 작으면 스왑
  		if self.items[i] < self.items[parent]:
  			self.items[parent], self.items[i] = self.items[i], self.items[parent]
  		i = parent
  		parent = i // 2

  def insert(self, k):
  	self.items.append(k)
  	self._percolate_up()
  ```

- 추출(Down-Heap) → percolate_down() → **heapq.heappop**

  1. 노드 값을 추출한 뒤, 제일 마지막 노드로 채운다.
  2. 자식 노드와 비교해 값이 더 큰 경우 위치를 변경한다.
  3. 재귀호출을 통해 자식값과 비교해 위치를 변경한다.

  ```python
  def _percolate_down(self, idx):
  	left = idx * 2
  	right = idx * 2 + 1
  	smallest = idx

  	if left < len(self) and self.items[left] < self.items[smallest]:
  		smallest = left
  	if right < len(self) and self.items[right] < self.items[smallest]:
  		smallest = right

  	if smallest != idx:
  		self.items[idx], self.items[smallest] = self.items[smallest], self.items[idx]
  		self._percolate_down[smallest]

  def extract(self):
  	extracted = self.items[1]
  	self.items[1] = self.items[len(self)]
  	self.items.pop()
  	self._percolate_down(1)
  	return extracted
  ```
