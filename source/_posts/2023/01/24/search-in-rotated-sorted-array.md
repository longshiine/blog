---
title: Leetcode 33. Search in Rotated Sorted Array
categories:
  - Tech
  - 알고리즘
tags: [BinarySearch]
date: 2023-01-24 20:58:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제66: 회전 정렬된 배열 검색

### 문제

---

특정 피벗을 기준으로 회전하여 정렬된 배열에서 target 값의 인덱스를 출력하라

### 입력

---

```python
nums = [4,5,6,7,0,1,2], target = 1
```

### 출력

---

```python

```

### 풀이

---

1. 이분 탐색으로 최댓값의 인덱스를 찾아, 원상태의 배열 만들기
2. 이분 탐색으로 O(log n)안에 찾기

```python

# 이분 탐색으로 Pivot Index 찾기
def pivot_search(left, right):
    k = (left + right) // 2
    if k != left and nums[left] > nums[right]:
        if nums[left] > nums[k]:
            return pivot_search(left, k)
        elif nums[k] > nums[right]:
            return pivot_search(k, right)
        else:
            return k # 최댓값 다음 값을 전달
    else:
        return k

# 이분 탐색으로 Target Index 찾기
def binary_search(nums, left, right):
    if left <= right:
        mid = (left+right)//2
        if nums[mid] > target:
            return binary_search(nums, left, mid-1)
        elif nums[mid] < target:
            return binary_search(nums, mid+1, right)
        else:
            return mid
    else:
        return -1

pivot = pivot_search(0, len(nums)-1)
nums1 = nums[pivot+1:]
nums2 = nums[:pivot+1]

index1 = binary_search(nums1, 0, len(nums1)-1)
index2 = binary_search(nums2, 0, len(nums2)-1)

if index1 == -1 and index2 == -1:
    return -1
elif index1 == -1:
    return index2
elif index2 == -1:
    return index1 + pivot + 1
```

### **새로운 개념**

---

-
