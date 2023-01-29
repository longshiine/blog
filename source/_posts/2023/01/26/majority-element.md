---
title: Leetcode 169. Majority Element
categories:
  - Tech
  - 알고리즘
tags: [Divide&Conquer]
date: 2023-01-26 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제83: 과반수 엘리먼트

### 문제

---

과반수를 차지하는(절반을 초과하는)엘리먼트를 출력하라.

### 입력

---

```python
[3,2,3]
[2,2,1,1,1,2,2]
```

### 출력

---

```python
3,
2
```

### 풀이

---

1. Counter 사용

```python
def majorityElement(self, nums):
	counts = collections.defaultdict(int)
	for num in nums:
		if counts[num] == 0:
			counts[num] = nums.count(num)
		if counts[num] > len(nums) // 2:
			return counts[nums]
```

2. 분할 정복

```python
def majorityElement(self, nums):
	if not nums:
		return None
	if len(nums) == 1:
		return nums[0]

	half = len(nums) // 2
	a = self.majorityElement(nums[:half])
	b = self.majorityElement(nums[half:])

	return [b, a][nums.count(a) > half]
```

### **새로운 개념**

---

-
