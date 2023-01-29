---
title: Leetcode 787. Cheapest Flights Within K Stops
categories:
  - Tech
  - 알고리즘
tags: [Dijkstra]
date: 2023-01-20 20:55:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제41: K경유지 내 가장 저렴한 항공권

> 흐으음… 컨셉은 맞췄는데..

### 문제

---

시작점에서 도착점까지의 가장 저렴한 가격을 계산하되, K개의 경유지 이내에 도착하는 가격을 리턴하라. 경로가 존재하지 않을 경우 -1을 리턴한다.

### 입력

---

```python
n=3, edges = [[0,1,100], [1,2,100], [0,2,500]]
src=0, dst=2 K=0
```

### 출력

---

```python
500
```

### 풀이

---

1. BSF로 풀이?
   1. 시작점에서 출발하여, 목적지까지를 향하되, 노드의 깊이를 K까지만 탐색한다
   2. 한 레벨의 노드를 전부 queue에 담고 차례로 빼낸다.

```python
graph = collections.defaultdict(list)
for u,v,w in edges:
	graph[u].append((v,w))

# 가격, 정점
Q = collections.deque()
Q.append((0, src))

# 최소 비용
dist = collections.defaultdict(None)

while Q and K > -1:
	cost, node = Q.popleft()
	for v,w in graph[node]:
		alt = cost + w
		if not dist[v] or dist[v] > alt:
			dist[v] = alt

	K -= 1

return dist[dst] if dist[dst] else -1
```

1. 우선순위 큐 사용

```python
graph = collections.defaultdict(list)
for u,v,w in flights:
    graph[u].append((v,w))

# 가격, 정점, 남은 경유지 수
Q = [(0, src, k)]

# 레벨 컨셉?
while Q:
    price, node, K = heapq.heappop(Q)
    if node == dst:
        return price
    if K >= 0:
        for v, w in graph[node]:
            alt = price + w
            heapq.heappush(Q, (alt, v, K-1))
return -1
```

### **새로운 개념**

---

-
