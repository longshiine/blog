---
title: Leetcode 215. Kth Largest Element in an Array
categories:
  - Tech
  - 알고리즘
tags: [Heap]
date: 2023-01-22 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제55: 배열의 K번째 큰 요소

### 문제

---

정렬되지 않은 배열에서 k번째 큰 요소를 추출하라.

### 입력

---

```python
[3,2,3,1,2,4,5,5,6], k = 4
```

### 출력

---

```python
4
```

### 풀이

---

1. 정렬 후 → 추출 = O(nlogn)

```python
return sorted(nums)[-k]
```

2. heapq 모듈 사용

```python
heapq.heapify(nums)

for _ in range(len(nums) - k):
	heapq.heappop(nums)

return heapq.heappop(nums)
```

### **새로운 개념**

---

- heapq는 최소 힙만을 지원한다.
- heapq.heapify(list) → 한번에 힙으로 만들어 줌
