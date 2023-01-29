---
title: Leetcode 104. Maximum Depth of Binary Tree
categories:
  - Tech
  - 알고리즘
tags: [Tree]
date: 2023-01-21 20:55:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제42: 이진트리의 최대 깊이

### 문제

---

이진 트리의 최대 깊이를 구하라.

### 입력

---

```python
[3,9,20,null,null,15,7]
```

### 출력

---

```python
3
```

### 풀이

---

1. 반복구조로 BFS

```python
if root is None:
	return 0
queue = collections.deque([root])
depth = 0

while queue:
	depth += 1
	for _ in range(len(queue)):
		cur_root = queue.popleft()
		if cur_root.left:
			queue.append(cur_root.left)
		if cur_root.right:
			queue.append(cur_root.right)

return depth
```

### **새로운 개념**

---

-
