---
title: Leetcode 234. Palindrome Linked List
categories:
  - Tech
  - 알고리즘
tags: [Linkedlist]
date: 2023-01-13 20:55:38
thumbnail: https://leetcode.com/static/images/LeetCode_Sharing.png
---

# 문제13: 팰린드롬 연결리스트

> **연결리스트의 경우에는 런너 활용 고민해보기!**

### 문제

---

연결리스트가 팰린드롬 구조인지 판별하라

### 입력

---

```python
1->2
1->2->2->1
```

### 출력

---

```python
False
True
```

### 풀이

---

1. 리스트로 만들고 O(n), 투포인터 풀이 O(n) → O(n)

```python
nums = []
while True:
    nums.append(head.val)
    if head.next != None:
        head = head.next
    else:
        break

left, right = 0, len(nums)-1
while left < right:
    if nums[left] == nums[right]:
        left += 1
        right -= 1
    else:
        return False
return True
```

1. 런너를 이용한 우아한 풀이
   1. 순서에 따라 2개의 런너, 빠른 런너와 느린 런너를 각각 출발시키면, 빠른런너가 끝에 다다를 때 느린 런너는 느린 런너는 정확히 중앙에 위치한다.
   2. 빠른 런너 → 홀/짝 판단, 느린 런너 → 역방향 팰린드롬 생성
   3. **리스트로 변환하지 않고, 연결리스트를 연결리스트 답게 풀이하는 방식!**

```python
rev = None
slow = fast = head
while fast and fast.next:
	fast = fast.next.next
	rev, rev.next, slow = slow, rev, slow.next
if fast:
	slow = slow.next

while rev and rev.val == slow.val:
	slow, rev = slow.next, rev.next
return not rev # 끝이면 True, 중간에 값이 있는 경우였으면 False

```

### **새로운 개념**

---

- **다중 할당**
  - **slow = fast = head 처럼 할당하는 방식**
  - rev, rev.next, slow = slow, rev, slow.next
    - rev, [rev.next](http://rev.next) = slow, rev
    - slow = slow.next
      로 할당하게 되면 rev=slow가 되며 동일 참조를 하게 된다.
- **런너 기법**
  - 연결리스트를 순회할 때 2개의 포인터를 동시에 사용하는 기법
  - 병합 지점, 중간 위치, 길이 등을 판별할 때 유용하게 사용할 수 있다.
  - fast = 2x, slow = 1x로 설정하면
    - 빠른 런너가 연결리스트의 끝에 정확히 도달하면 (홀수 길이)
      - 느린 런너는 정확히 연결리스트의 중간지점을 가리키게 됨
      - 1→2→3→2→1
      - s f
    - 만약 넘치면, 느린 런너는
      - 느린 런너는 중간 두개중 하나를 가리키게 됨
      - 1→2→2→1 None
      - s f
- 별도 개념이긴 한데, 무한대 값을 설정하는 방법은 다음과 같다.
  - mx = sys.maxsize
  - mn = -sys.maxsize
  - mx = float(’inf’)
  - mn = float(’-inf’)
