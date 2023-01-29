---
title: Leetcode 509. Fibonacci Number
categories:
  - Tech
  - 알고리즘
tags: [DynamicProgramming]
date: 2023-01-27 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 85번: 피보나치 수

> 재귀와 다이나믹 프로그래밍을 동시에 물어볼 수 있는 문제.

### 문제

---

피보나치 수를 구하라 f(n) = f(n-1)+f(n-2)

0, 1, 1, 2, 3, 5, 8, 13 …

### 입력

---

```python

```

### 출력

---

```python

```

### 풀이

---

- 만일 N=5라면

1. 재귀를 이용한 풀이 (15번 연산)

```python
def fib(n):
	if n <= 1:
		return n
	return fib(n-1)+fib(n-2)
```

2. 다이나믹 프로그래밍
   a. **하향식: 메모이제이션** (9번 연산)

   ```python
   def fib(n):
   	if n <= 1:
   		return n

   	if self.dp[n]:
   		return self.dp[n]
   	self.dp[n] = self.dp[n-1] + self.fib(n-2)
   	return self.dp[n]
   ```

   b. **상향식: 타뷸레이션 (n번 연산) → O(n)**

   ```python
   def fib(n):
   	self.dp[0] = 0
   	self.dp[1] = 1

   	for i in range(2, n+1):
   		self.dp[i] = self.dp[i-1]+self.dp[i-2]

   	return self.dp[n]
   ```

3. 두 변수만 이용해 공간절약

```python
def fib(n):
	x,y = 0,1
	for i in range(0, n):
		x,y = y, x+y
	return x
```

### **새로운 개념**

---

-
