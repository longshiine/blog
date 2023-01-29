---
title: Leetcode 1. Two Sum
categories:
  - Tech
  - 알고리즘
tags: [List]
date: 2023-01-12 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제7: 두 수의 합

> 여러가지 풀이가 있으니, 문제를 정말 잘 읽는게 중요하겠다.
> 있는지 여부를 리턴하는지, 인덱스를 리턴하는지에 대해 생각해 볼 필요가 있다.

### 문제

---

덧셈하여 타겟을 만들 수 있는 배열의 두 숫자 인덱스를 리턴하라.

### 입력

---

```python
nums = [2, 7, 11, 15], target = 9
```

nums = [7, 2, 15, 11]

### 출력

---

```python
[0, 1]
```

### 풀이

---

1. 이 배열은 정렬되어 있는가? (Yes)

   1. 반복문 두개로 푼다 O(n^2)
   2. 이진 탐색으로 푼다 O(nlogn)
   3. 양쪽을 가르키는 포인터를 두고 하나씩 줄여나간다. O(n)
      1. 투포인터 구현

   ```python
   l = 0
   r = len(nums) - 1
   while l != r:
      if nums[l]+nums[r] == target:
         return [l, r]
      elif nums[l]+nums[r] < target:
         l += 1
         continue
      elif nums[l]+nums[r] > target:
         r -= 1
         continue
   return False
   ```

2. 이 배열은 정렬되어 있는가? (No)

   a. 반복문 두개로 푼다 O(n^2) → **브루트 포스**

   ```python
   for i in range(0, len(nums)):
   	for j in range(i+1, len(nums)):
   		if nums[i]+nums[j] == target and i != j:
            return [i, j]
   return False
   ```

   b. **첫번째 수를 뺀 결과 키 조회 (상수 복잡도를 가지는 해시테이블 조회를 사용)**

   ```python
   num_dict = {}
   for i, num in enumerate(nums):
   	num_dict[num] = i

   for i, num in enumerate(nums):
   	if target - num in num_dict and i != num_dict[target - num]:
   		return [i, num_dict[target-num]]
   ```

   - for문 대신 해시 테이블 조회를 사용한 것인데, 최악의 경우가 O(n)이고, 분할 상환 분석에 따른 시간 복잡도는 O(1)이다. → constant time lookup
   - **자료구조에 대한 이해가 매우 중요한 문제 + 딕셔너리 활용에 대한 기발한 아이디어**

   c. O(nlogn) 풀이

   1. 정렬
   2. 투 포인터로 값 찾기 O(n)
   3. 인덱스 찾기 O(n)

   ```python
   s_nums = sorted(nums)

   l = 0
   r = len(s_nums) - 1
   arr = []

   while l != r:
      if s_nums[l]+s_nums[r] == target:
         arr = [s_nums[l], s_nums[r]]
         break
      elif s_nums[l]+s_nums[r] < target:
         l += 1
         continue
      elif s_nums[l]+s_nums[r] > target:
         r -= 1
         continue

   answer = []
   for i, num in enumerate(nums):
      if num == arr[0]:
         answer.append(i)
         arr[0] = None
      elif num == arr[1]:
         answer.append[i]
         arr[1] = None

   return answer
   ```

### **새로운 개념**

---

- key in nums_map : **해시 테이블 조회는 상수 시간 내에 가능하다!**
