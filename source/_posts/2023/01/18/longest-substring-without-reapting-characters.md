---
title: Leetcode 3. Longest Substring Without Repeating Characters
categories:
  - Tech
  - 알고리즘
tags: [HashTable]
date: 2023-01-18 20:55:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제30: 중복 문자 없는 가장 긴 문자열

### 문제

---

중복 문자가 없는 가장 긴 부분 문자열의 길이를 리턴하라

### 입력

---

```python
"abcabcbb"
"bbbbbb"
```

### 출력

---

```python
3
1
```

### 풀이

---

1. 브루트 포스 풀이 = O(n^2)
   1. 중복 문자 없는 부분 문자열로 dict 만들기 → 길이가 key, 문자열이 value

```python
count = collections.defaultdict(str)
for i in range(len(s)+1): # 부분 문자열 길이 반복
    for j in range(len(s)): # 문자열 내 반복
        sub = s[j:j+i]
        if i == len(set(sub)):
            count[i] = sub
            break
    if i not in count:
        break

return max(count.keys()) if count.keys() else 0
```

1. 슬라이딩 윈도우와 투포인터로 사이즈 조절
   1. 만약 중복 문자가 없는 경우 사이즈를 키우고, 중복문자가 있다면 시작 포인터를 다음으로 넘기는 형태

```python
front, rear, length = 0, 1, 0

for i in range(len(s)):
	sub = s[front:rear]
	if len(sub) != len(set(sub)): # 중복 문자 검사
		front += 1
	else:
		length += 1
	rear += 1

return length
```

### **새로운 개념**

---

-
