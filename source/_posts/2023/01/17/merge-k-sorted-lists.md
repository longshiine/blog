---
title: Leetcode 23. Merge K Sorted Lists
categories:
  - Tech
  - 알고리즘
tags: [PriorityQueue]
date: 2023-01-17 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제27: k개 정렬 리스트 병합

### 문제

---

k개의 정렬된 리스트를 1개의 정렬된 리스트로 병합하라.

### 입력

---

```python
[
	1->4->5,
	1->3->4,
	2->6,
]
```

### 출력

---

```python
1->1->2->3->4->4->5->6
```

### 풀이

---

1. 우선순위 큐를 이용한 리스트 병합

```python
root = result = ListNode(None)
heap = []

for i in range(len(lists)):
	if lists[i]:
		heapq.heappush(heap, (lists[i].val, i, lists[i]))

while heap:
	node = heapq.heappop(heap)
	idx = node[1]
	result.next = node[2]

	result = result.next
	if result.next:
		heapq.heappush(heap, (result.next.val, idx, result.next))
```

### **새로운 개념**

---

- 파이썬에서 우선순위 큐는 queue 모듈의 PriorityQueue 클래스를 이용해 사용할 수 있다.
  - 스레드 세이프를 보장하지만, 굳이 멀티 스레드로 구현할 게 아니라면 기능이 똑같기 때문에 사용할 필요가 없다.
- 실무에서도 우선순위 큐는 대부분 heapq로 구현한다.
