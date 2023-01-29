---
title: <파이썬 알고리즘 인터뷰> - 22장. 분할 정복
categories:
  - Tech
  - 알고리즘
tags: [Divide&Conquer]
date: 2023-01-26 20:50:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a>

{% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 22장: 분할정복

> 분할 정복은 다중 분기 재귀를 기반으로 하는 알고리즘 디자인 패러다임을 이야기한다.

- 분할 정복은 직접 해결할 수 있을 정도로 간단한 문제가 될 때까지 문제를 재귀적으로 쪼개나간 다음, 그 하위 문제의 결과들을 조합하여 원래 문제의 결과로 만들어낸다.
- 대표적인 알고리즘으로는 **병합정렬**

- **분할: 문제를 동일한 유형의 여러 하위 문제로 나눈다.**
- **정복: 가장 작은 단위의 하위 문제를 해결하여 정복한다.**
- **조합: 하위 문제에 대한 결과를 원래 문제에 대한 결과로 조합한다.**

```python
function F(x):
	if F(x)가 간단 then:
		return F(x) # 정복
	else:
		x를 x1, x2로 분할
		F(x1)과 F(x2)를 호출
		return F(x1), F(x2)로 F(x)를 구한 값 # 조합, 분할
```
