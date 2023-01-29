---
title: Leetcode 206. Reverse Linked List
categories:
  - Tech
  - 알고리즘
tags: [Linkedlist]
date: 2023-01-13 20:56:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제15: 역순 연결리스트

### 문제

---

연결 리스트를 뒤집어라

### 입력

---

```python
1->2->3->4->5->NULL
```

### 출력

---

```python
5->4->3->2->1->NULL
```

### 풀이

---

1. O(n) 풀이
   1. 반복 한번으로 역순을 만든다.

```python
if head == None: return None
result = copy.copy(head)
result.next = None
head = head.next
while head:
    temp = copy.copy(head)
    temp.next = result
    result = temp
    head = head.next

return result
```

1. 재귀를 활용한 풀이
   1. 헤드값만 리턴하면 되므로,

```python
def reverse(node: ListNode, prev: ListNode = None):
	if not node:
		return prev
	next, node.next = node.next, prev
	return reverse(next, node)

return reverse(head)
```

1. 반복을 활용한 풀이

```python
node, prev = head, None

while node:
	next, node.next = node.next, prev
	prev, node = node, next

return prev
```

### **새로운 개념**

---

-
