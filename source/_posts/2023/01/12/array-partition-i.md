---
title: Leetcode 561. Array Partition (I)
categories:
  - Tech
  - 알고리즘
tags: [List]
date: 2023-01-12 20:58:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제10: 배열 파티션(I)

> 귀납적으로 오름차순 시 옳다는 것을 알긴했는데, 증명까진 못했다.

### 문제

---

n개의 페어를 이용한 min(a, b)의 합으로 만들 수 있는 가장 큰 수를 출력하라

### 입력

---

```python
[1,4,3,2]
```

### 출력

---

```python
4
```

### 풀이

---

1. 정렬 후 오름차순 풀이 O(nlogn)
   1. 정렬 한 뒤 첫번째 항들만 더한다.

```python
nums.sort()
result = 0
for i in range(0, len(nums)-1, 2):
	result += nums[i]

return result
```

1. 정렬 후 오름차순 풀이 → 파이썬 다운 방식 → 슬라이싱 활용
   1. sorted(nums)[::2]

```python
return sum(sorted(nums)[::2]) # 짝수 번째 인덱스만 활용
```

### **새로운 개념**

---

- sum(sorted(nums)[::2]) → 코드가 세련되다..
