---
title: Leetcode 77. Combinations
categories:
  - Tech
  - 알고리즘
tags: [Graph]
date: 2023-01-19 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제35: 조합

> 조합은 빈 배열에 더해가며, 순열은 있는 배열에서 제거해가며

### 문제

---

전체 수 n을 입력받아 k개의 조합을 리턴하라

### 입력

---

```python
n = 4, k = 2
```

### 출력

---

```python
[
	[1,2],
	[1,3],
	[1,4],
	[2,3],
	[2,4],
	[3,4]
]
```

### 풀이

---

1. dfs 활용, 재귀 풀이

```python
results = []

def dfs(elements, start, k):
	if k == 0:
		results.append(elements[:])
		return
	for i in range(start, n+1):
		elements.append(i)
		dfs(i+1, k-1)
		elements.pop()

dfs([], 1, k)
return results
```

2. itertools 이용

```python
results =
return list(itertools.combinations(range(1, n+1), k))
```

3. 내 풀이

```python
def solution(nums):
	results = []
	def dfs(p, items):
		if len(p)==k:
			results.append(p)
		else:
			for idx in range(len(items)):
				# 순열 풀이와 남아있는 배열 이용하는 방식만 다르게!
				dfs(p+[items[idx]], items[idx+1:])
	dfs([], [i for i in range(1,n+1)])
	return results


```

### **새로운 개념**

---
