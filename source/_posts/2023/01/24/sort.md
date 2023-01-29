---
title: <파이썬 알고리즘 인터뷰> - 17장. 정렬 & 18장. 이진검색
categories:
  - Tech
  - 알고리즘
tags: [Sort, BinarySearch]
date: 2023-01-24 20:50:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a>

{% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 17장: 정렬

> 정렬 알고리즘은 목록의 요소를 특정 순서대로 넣는 알고리즘이다. 대개 숫자식 순서와 사전식 순서로 정렬한다.

### 버블 정렬

---

```python
def bubble_sort(nums):
	for i in range(1, len(nums)):
		for j in range(0, len(nums)-1):
			if nums[j] > nums[j+1]:
				nums[j], nums[j+1] = nums[j+1], nums[j]
```

### 퀵정렬

---

```python
def quicksort(A, lo, hi):
	def partition(lo, hi):
		pivot = A[hi]
		left = lo
		for right in range(lo, hi):
			if A[right] < pivot:
				A[left], A[right] = A[right], A[left]
				left += 1
		A[left], A[hi] = A[hi], A[left]
		return left


	if lo < hi:
		pivot = partition(lo, hi)
		quicksort(A, lo, pivot-1)
		quicksort(A, pivot+1, hi)

```

# 18장: 이진 검색

> 이진 검색이란 정렬된 배열에서 타겟을 찾는 검색 알고리즘이다.

- 이진 검색은 값을 찾아내는 시간 복잡도가 O(log n)이라는 점에서 대표적인 로그 시간 알고리즘이며, 이진 탐색 트리(BST)와도 유사한 점이 많다.
- BST가 정렬된 구조를 저장하고 탐색하는 자료구조라면, 이진 검색은 정렬된 배열에서 값을 찾아내는 알고리즘 자체를 지칭한다.
