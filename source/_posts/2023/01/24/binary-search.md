---
title: Leetcode 704. Binary Search
categories:
  - Tech
  - 알고리즘
tags: [BinarySearch]
date: 2023-01-24 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제65: 이진 검색

### 문제

---

정렬된 nums를 입력받아 이진 검색으로 target에 해당하는 인덱스를 찾아라.

### 입력

---

```python
nums = [-1,0,3,5,9,12], target=9
```

### 출력

---

```python
4
```

### 풀이

---

1. 재귀를 이용한 풀이

```python
def binary_search(left, right):
	if left <= right:
		mid = (left + right) // 2

		if nums[mid] < target:
			return binary_search(mid+1, right)
		elif nums[mid] > target:
			return binary_search(left, mid-1)
		else:
			return mid
	else:
		return -1

return binary_search(0, len(nums)-1)
```

2. 반복을 이용한 풀이

```python
left, right = 0, len(nums)-1
while left <= right:
	mid = (left + right) // 2
	if nums[mid] < target:
		left = mid + 1
	elif nums[mid] > target:
		right = mid - 1
	else:
		return mid
return -1
```

3. 모듈을 사용한 풀이

```python
index = bisect.bisect_left(nums, target)

if index < len(nums) and nums[index] == target:
	return index
else:
	return -1
```

### **새로운 개념**

---

-
