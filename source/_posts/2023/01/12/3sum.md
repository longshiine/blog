---
title: Leetcode 15. 3Sum
categories:
  - Tech
  - 알고리즘
tags: [List]
date: 2023-01-12 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제9: 세 수의 합

> 아이디어는 통했다. 그러나 구현단이 아직 많이 미숙하다.. 많이 하면서 친숙해져야 한다.

### 문제

---

배열을 입력받아 합으로 0을 만들 수 있는 3개의 엘리먼트를 출력하라

### 입력

---

```python
nums = [-1, 0, 1, 2, -1, -4]
```

### 출력

---

```python
[
	[-1, 0, 1],
	[-1, 2, -1]
]
```

### 풀이

---

1. 브루트포스 풀이 O(n^3)
   1. 이 경우 시간 에러날 듯
2. for문 한 개와 투포인터 O(n^2)
   1. 정렬 O(nlogn)
   2. for문으로 하나를 잡고, 투포인터를 돌면서 좌측의 엘리먼트의 역값을 타겟으로 삼음
   3. 중복되는 요소가 없어야 함으로 for문 돌 때 조건 추가

```python
nums.sort()
results = []

for i in range(len(nums)-2):
    if i > 0 and nums[i] == nums[i-1]:
        continue
    left, right = i+1, len(nums)-1
    while left < right:
        sum = nums[left]+nums[right]+nums[i]
        if sum < 0:
            left += 1
            continue
        elif sum > 0:
            right -= 1
            continue
        else:
            output.append([nums[i], nums[left], nums[right]])
            while left < right and nums[left] == nums[left+1]:
                left += 1
            while left < right and nums[right] == nums[right-1]:
                right -= 1
            left += 1
            right -= 1
return results
```

### **새로운 개념**

---

- left, right = i+1, len(nums)-1
  - 한번에 할당하기
- 투 포인터 기법
  - 지속적으로 등장하는데, **대개 정렬되어 있을 때 유용하다.**
  - 왼쪽 포인터와 오른쪽 포인터 두 지점을 기준으로 하는 문제 풀이 전략이다.
