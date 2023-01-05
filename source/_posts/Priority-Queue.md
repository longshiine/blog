---
title: Priority Queue(우선순위큐)
date: 2021-02-05 21:46:32
categories:
  - Tech
  - 알고리즘
tags: [Algorithm, PriorityQueue, heapq]
---

> 프로그래머스: 이중우선순위 큐 풀이
> 코테 대비 후루룩 훑기(2)
> https://programmers.co.kr/learn/courses/30/lessons/42628

{% img /image/post_image/20210205/write2.jpeg 'width:550px; height:600px' '' '힙, 우선순위큐 필기' %}

{% img /image/post_image/20210205/dual.png 'width:600px; height:700px' '' '이중우선순위큐' %}

```python
def solution(operations):
    import heapq
    length = 0
    heap = []
    for operation in operations:
        oper = operation.split()
        if oper[0] == "I":
            heapq.heappush(heap, int(oper[1]))
            length += 1
        if oper[0] == "D" and len(heap) != 0:
            if oper[1] == "-1":
                heapq.heappop(heap)
                length -= 1
            else:
                # 여기가 중요한데, heapq는 최소힙이므로,
                # 최댓값을 찾기 위해서, 힙에 있는 요소들을 pop한뒤 새로운 heap에 push한다.
                # 단, 마지막에 pop 되는 값이 최댓값이므로 이 값을 버린다.
                new_heap = []
                for _ in range(length-1):
                    heapq.heappush(new_heap, heapq.heappop(heap))
                heap = new_heap
                length -= 1
    if length != 0:
        answer = [max(list(heap)), min(list(heap))]
    else:
        answer = [0,0]
    return answer
```

### heapq 모듈 정리

- heapq는 기본적으로 **최소힙** 을 지원
- `heapq.heappush(heap, node)`: 힙에 요소를 추가
- `heapq.heappop(heap)`: 힙에서 최솟값을 제거 (이때 자동으로 힙 정렬이루어짐)
- `heapq.heapify(list)`: 리스트를 heap으로 만들어줌
