---
title: Leetcode 20. Valid Parentheses
categories:
  - Tech
  - 알고리즘
tags: [Stack]
date: 2023-01-16 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제20: 유효한 괄호

> 스택 구조에 대한 제일 기본적인 문제

### 문제

---

괄호로 된 입력값이 올바른지 판별하라

### 입력

---

```python
()[]{}
([])
```

### 출력

---

```python
true
```

### 풀이

---

1. ‘(’, ‘[’, ‘{’는 스택에 push하고 ‘)’, ‘]’, ‘}’를 만날때 pop한다.

```python
stack = []
table = {
    ')': '(',
    ']': '[',
    '}': '{',
}

for char in s:
    if char not in table:
        stack.append(char)
    elif not stack or table[char] != stack.pop():
        return False

return len(stack) == 0
```

### **새로운 개념**

---

- 스택에 푸쉬하고, 만날때 pop하는 컨셉
