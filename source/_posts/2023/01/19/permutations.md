---
title: Leetcode 46. Permutations
categories:
  - Tech
  - 알고리즘
tags: [Graph]
date: 2023-01-19 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제34: 순열

> 재귀 컨셉으로 순열 푸는게 어렵다..

### 문제

---

서로 다른 정수를 입력받아 가능한 모든 순열을 리턴하라.

### 입력

---

```python
[1,2,3]
```

### 출력

---

```python
[
	[1,2,3],
	[1,3,2],
	[2,1,3],
	[2,3,1],
	[3,1,2],
	[3,2,1],
]
```

### 풀이

---

1. 재귀를 활용한 풀이
   1. $A_{3}^{0}, A_{1}^{3}, A_{3}^{3}, A_{3}^{3},$ 을 계산하기

```python
results = []
prev_elements = []

def dfs(elements):
	if len(elements) == 0:
		results.append(prev_elements[:]) # prev_elements[:]로 안하면 참조가 append된다.
	for e in elements:
		next_elements = elements[:]
		next_elements.remove(e)

		prev_elements.append(e)
		dfs(next_elements)
		prev_elements.pop()

dfs(nums)
return results

```

2. itertools을 활용한 풀이

```python
return list(map(itertools.permutations(nums)))
```

3. 내 풀이

```python
def solution(nums):
	results = []
	def dfs(p, items):
		if len(p)==len(nums):
			results.append(p)
		else:
			for idx in range(len(items)):
				dfs(p+[items[idx]], items[:idx]+items[idx+1:])
	dfs([], nums)
	return results
```

### **새로운 개념**

---

- list(itertools.permutations(nums)) → 구현의 효율성, 성능을 위해서 사용
  - 리스트 내의 튜플로 반환 되므로, 리스트로 변환 필요
