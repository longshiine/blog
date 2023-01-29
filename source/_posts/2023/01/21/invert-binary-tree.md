---
title: Leetcode 224. Invert Binary Tree
categories:
  - Tech
  - 알고리즘
tags: [Tree]
date: 2023-01-21 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제45: 이진 트리 반전

### 문제

---

이진트리를 반전하라

### 입력

---

```python
[4,2,7,1,3,6,9]
```

### 출력

---

```python
[4,7,2,9,6,3,1]
```

### 풀이

---

1. bfs로 내려가면서 level 별로 dict에 저장 → list를 반전시키고 트리 생성

```python
풀이집에도 있는 풀이였다! 물론 list 생성은 하지 않지만!
```

1. 재귀로 자식 두개를 바꾸는 것 반복

```python
def dfs(node):
	if node is None:
		return
	dfs(node.left)
	dfs(node.right)

	node.left, node.right = node.right, node.left

```

1. 더 짧게

```python
def invertTree(root):
	if root:
		root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
	return root
```

### **새로운 개념**

---

-
