---
title: Leetcode 200. Number of Islands
categories:
  - Tech
  - 알고리즘
tags: [Graph]
date: 2023-01-19 20:55:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제32: 섬의 개수

### 문제

---

1을 육지로, 0을 물로 가정한 2D 그리드 맵이 주어졌을때, 섬의 개수를 계산하라.

### 입력

---

```python
11110
11010
11000
00000

11000
11000
00100
00011
```

### 출력

---

```python
1
3
```

### 풀이

---

1. 그래프 제작후 dfs

```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        def dfs(i, j):
            if i < 0 or i >= len(grid) or j < 0 or j >= len(grid[0]) or grid[i][j] != '1':
                return

            grid[i][j] = '0'

            dfs(i-1, j)
            dfs(i+1, j)
            dfs(i, j-1)
            dfs(i, j+1)

        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(i,j)
                    count += 1

        return count
```

### **새로운 개념**

---

- 중첩함수
  - 부모의 변수를 공유!
