---
title: Leetcode 622. Design-Circular-Queue
categories:
  - Tech
  - 알고리즘
tags: [Queue]
date: 2023-01-16 20:57:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제25: 원형 큐 디자인

### 문제

---

원형 큐를 디자인하라.

### 입력

---

```python
circularQueue.enQueue(10)
```

### 출력

---

```python
True
```

### 풀이

---

1. enQueue, deQueue, Front, Rear, isFull, isEmpty 구현

```python
class MyCircularQueue(object):

    def __init__(self, k):
        """
        :type k: int
        """
        self.q = [None] * k
        self.max = k
        self.front = 0
        self.rear = 0

    def enQueue(self, value):
        """
        :type value: int
        :rtype: bool
        """
        if self.q[self.rear] is None:
            self.q[self.rear] = value
            self.rear = (self.rear + 1) % self.max
            return True
        else:
            return False


    def deQueue(self):
        """
        :rtype: bool
        """
        if self.q[self.front] is None:
            return False
        else:
            self.q[self.front] = None
            self.front = (self.front + 1) % self.max
            return True

    def Front(self):
        """
        :rtype: int
        """
        return -1 if self.q[self.front] is None else self.q[self.front]

    def Rear(self):
        """
        :rtype: int
        """
        return -1 if self.q[self.rear - 1] is None else self.q[self.rear - 1]

    def isEmpty(self):
        """
        :rtype: bool
        """
        return self.front == self.rear and self.q[self.front] is None

    def isFull(self):
        """
        :rtype: bool
        """
        return self.front == self.rear and self.q[self.front] is not None


# Your MyCircularQueue object will be instantiated and called as such:
# obj = MyCircularQueue(k)
# param_1 = obj.enQueue(value)
# param_2 = obj.deQueue()
# param_3 = obj.Front()
# param_4 = obj.Rear()
# param_5 = obj.isEmpty()
# param_6 = obj.isFull()
```

### **새로운 개념**

---

- 원형 큐 = 환형 큐 = 링 버퍼
