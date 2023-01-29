---
title: Leetcode 938. Range Sum of BST
categories:
  - Tech
  - 알고리즘
tags: [Tree, BST]
date: 2023-01-21 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제52: 이진 탐색 트리(BST) 합의 범위

### 문제

---

이진 탐색 트리(BST)가 주어졌을 때 L이상 R이하의 값을 지닌 노드의 합을 구하라.

### 입력

---

```python
root = [10,5,15,3,7,null,18], L = 7, R = 15
```

### 출력

---

```python
32
```

### 풀이

---

1. 재귀 구조 dfs + 가지치기

```python
class Solution(object):
  result = 0
  def rangeSumBST(self, root, low, high):
    """
    :type root: TreeNode
    :type low: int
    :type high: int
    :rtype: int
    """
    def dfs(node):
      if node is None:
              return

      if node.val > low:
          left = dfs(node.left)
      if node.val <= high:
          right = dfs(node.right)

      if node.val >= low and node.val <= high:
          self.result += node.val

    dfs(root)
    return self.result
```

### **새로운 개념**

---

- 재귀 구조 쓸때는 그냥 주어진 함수로 푸는 것도 좋을 듯!
