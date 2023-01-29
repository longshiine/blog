---
title: <파이썬 알고리즘 인터뷰> - 7장. 배열
categories:
  - Tech
  - 알고리즘
tags: [List]
date: 2023-01-12 20:55:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a>

{% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 7장: 배열

> 배열은 값 또는 변수 엘리먼트의 집합으로 구성된 구조로, 하나 이상의 인덱스 또는 키로 식별된다.

자료구조는 크게 아래와 같이 나뉘며,

- **메모리 공간 기반의 연속 방식 → 배열**
- **포인터 기반의 연결 방식 → 연결리스트**

추상 자료형(ADT: Abstract Data Type, 스택 & 큐 등)의 실제 구현 대부분은

배열 혹은 연결 리스트를 기반으로 한다.

- 32bit 시스템에서 int는 4byte(32bit)
- 파이썬은 동적 배열(list) 자료구조를 지원하고, 정적 배열을 따로 지원x
