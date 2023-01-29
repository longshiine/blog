---
title: <파이썬 알고리즘 인터뷰> - 12장. 그래프
categories:
  - Tech
  - 알고리즘
tags: [Graph]
date: 2023-01-19 20:50:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a>

{% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 12장: 그래프

> 수학에서, 좀 더 구체적으로 그래프 이론에서 그래프란 객체의 일부 쌍들이 연관되어 있는 객체 집합구조를 말한다.

### **오일러 경로**

---

- 쾨니히스베르크 다리 문제 → 한개의 섬 7개의 다리 → 한번씩만 모두 건널 수 있을까?
- Vertex(정점)과 Edge(간선)
- 모든 정점이 짝수개의 차수를 갖는다면 모든 다리를 한 번씩만 건너서 도달하는 것이 성립한다.
  - 이를 오일러 경로라고 하는데, 쾨니히스베르크 다리는 오일러 경로가 아니다

### 해밀턴 경로

---

> 해밀턴 경로는 각 정점을 한 번씩 방문하는 무향 또는 유향 그래프 경로를 말한다.

- 오일러 경로는 간선을 기준으로 하고 해밀턴 경로는 정점을 기준으로 한다.
- 단순한 차이에도 불구하고 해밀턴 경로를 찾는 문제는 최적 알고리즘이 없는 대표적인 NP-완전 문제다/\
- 원래의 출발점으로 돌아오는 경로는 특별히 해밀턴 순환이라 하는데, 이중에서도 특히 최단거리를 찾는 문제는 알고리즘 분야에서는 외판원 문제(Travelling Salesman Problem; TSP)로도 더욱 유명하다.
  - 각 도시를 방문하고 돌아오는 가장 짧은 경로를 찾는 문제
  - 해밀턴 경로 > 해밀턴 순환 > 외판원 문제

### 그래프 순회

---

> 그래프 순회란 그래프 탐색이라고도 불리우며 그래프의 각 정점을 방문하는 과정을 말한다.

- DFS (Depth-First Search, 깊이 우선 탐색)
  - 주로 많이 쓰이고, 스택 혹은 재귀로 구현하며 백트래킹을 통해 뛰어난 효용을 보인다.
  - 재귀로 구현시 순서대로, 스택으로 구현시 역순으로 출력된다.
  ```python
  def iterative_dfs(start_v):
  	discovered = []
  	stack = [start_v]
  	while stack:
  		v = stack.pop()
  		if v not in discovered:
  			discovered.append(v)
  			for w in graph[v]:
  				stack.append(w)
  return discovered
  ```
- BFS (Breadth-First Search, 너비 우선 탐색)

  - 큐로 구현하며, 그래프의 최단 경로를 구하는 문제 등에 사용된다.
  - BFS는 재귀로 구현 불가하다.
  - 데크 사용 및 popleft() 사용시 더 최적화 가능

  ```python
  def iterative_bfs(start_v):
  	discovered = [start_v]
  	queue = [start_v]
  	while queue:
  		v = queue.pop(0)
  		for w in graph[v]:
  			if w not in discovered:
  				discovered.append(w)
  				queue.append(w)

  return discovered
  ```

- 그래프를 표현하는 방식
  - 인접 행렬
  - 인접 리스트
    - 출발 노드를 키로, 도착 노드를 값으로 표현 → 딕셔너리 자료형

### 백트래킹

---

> 백트래킹은 해결책에 대한 후보를 구축해 나아가다 가능성이 없다고 판단되는 즉시 후보를 포기(백트랙)해 정답을 찾아가는 범용적인 알고리즘으로 제약 충족 문제에 특히 유용하다.

- 백트래킹은 주로 재귀로 구현하며, DFS는 백트래킹의 골격을 이루는 알고리즘이다.
- DFS로 탐색을 시도하고 가능성이 없는 후보는 즉시 포기하는 등의 트리 탐색 등

### 제약 충족 문제

---

> 제약 충족 문제란 수많은 제약조건을 충족하는 상태를 찾아내는 수학문제를 일컫는다

- 스도쿠처럼 1에서 9까지 숫자를 한 번만 넣은 정답을 찾아내는 모든 문제 유형
  - 스도쿠를 풀이하려면 백트레킹을 하면서 가지치기를 통해 최적화하는 형태로 풀이할 수 있다.
