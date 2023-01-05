---
title: DFS와 BFS
date: 2021-02-05 13:50:29
categories:
  - Tech
  - 알고리즘
tags: [Algorithm, DFS, BFS]
---

> 백준 1260번 문제: DFS와 BFS 풀이
> 코테 대비 후루룩 훑기(1)
> https://www.acmicpc.net/problem/1260

{% img /image/post_image/20210205/write.jpeg 'width:650px; height:600px' '' 'DFS와 BFS 필기' %}

{% img /image/post_image/20210205/1260.png 'width:650px; height:500px' '' 'DFS와 BFS' %}

### 그래프 만들기

```python
if __name__ == "__main__":
    from sys import stdin
    N, E, begin = map(int, stdin.readline().rstrip().split()) # 입력 첫째줄 (노드개수, 간선개수, 시작점)
    graph = {node: [] for node in range(1, N+1)}
    i = 0

    while i < E:
        i += 1
        node, edge = map(int, stdin.readline().rstrip().split())
        graph[node].append(edge)
        graph[edge].append(node)

    print(dfs(graph, begin))
    print(bfs(graph, begin))
```

### DFS

```python
def dfs(graph, start_node):
    for n in graph: graph[n] = sorted(graph[n], reverse=True)
    visit = list()
    stack = list()
    stack.append(start_node)
    while stack:
        node = stack.pop()
        if node not in visit:
            visit.append(node)
            stack.extend(graph[node])
    return visit
```

### BFS

```python
def bfs(graph, start_node):
    for n in graph: graph[n] = sorted(graph[n])
    visit = list()
    stack = list()
    stack.append(start_node)
    while stack:
        node = stack.pop(0)
        if node not in visit:
            visit.append(node)
            stack.extend(graph[node])
    return visit
```

### 백준에서 파이썬 입출력 받기

```python
if __name__ == "__main__":
    from sys import stdin
    # .split()으로 input으로 들어오는 line을
    # 공백 단위의 리스트로 만들어주고,
    # .map(func, list)으로 각 데이터를 처리
    # 이때 반환되는 것은 "맵 객체" 이고, 여러 변수로 받을 수 있음
    N, M = map(int, stdin.readline().rstrip().split())

    while i < N: # 뭐 이런식으로 입력 더 받으면 됨
        A, B = map(int, stdin.readline().rstrip().split())
        i += 1
```
