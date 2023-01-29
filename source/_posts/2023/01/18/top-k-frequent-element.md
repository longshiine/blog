---
title: Leetcode 347. Top K Frequent Elements
categories:
  - Tech
  - 알고리즘
tags: [HashTable]
date: 2023-01-18 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제31: 상위 k 빈도 요소

### 문제

---

상위 k번 이상 등장하는 요소를 추출하라.

### 입력

---

```python
nums = [1,1,1,2,2,3], k = 2
```

### 출력

---

```python
[1,2]
```

### 풀이

---

1. 카운터 사용

```python
freqs = collections.Counter(nums)
s = sorted(freqs.items(), key=lambda x: -1 * x[1])
return [item[0] for item in s[:k]]
```

1. 파이썬 다운 방식

```python
return list(zip(*collections.Counter(nums).most_common(k)))[0]
```

### **새로운 개념**

---

- **collections.Counter(list).most_common(k)** = 빈도수가 높은 순서대로 아이템을 추출하는 기능 제공
- **zip() = 2개 이상의 시퀀스를 짧은 길이를 기준으로 일대일 대응하는 새로운 튜플 시퀀스를 반환**

```python
a = [1,2,3,4,5]
b = [2,3,4,5]
c = [3,4,5]

zip(a,b,c)
= [(1,2,3), (2,3,4), (3,4,5)] # C를 기준으로 엮임!
```

- **아스테리스크(\*) = 튜플 또는 리스트 등의 시퀀스 unpacking 시에 사용.**

```python
a = [1,2,3,4]
print(a) # [1,2,3,4]
print(*a) # 1 2 3 4
```

- **두개짜리(**) = 키, 값 페어를 unpacking\*\*

```python
>>> date_info = {'year': '2020', 'month': '01', 'day': '7'}
>>> new_info = {**date_info, 'day': '14'}
>>> new_info
{'year': '2020', 'month': '01', 'day': '14'}
```
