---
title: Gradient Vanishing이란?
date: 2020-08-27 16:07:41
categories:
  - Tech
  - 딥러닝
tags: [DeepLearning, GradientVanishing]
mathjax: true
---

Gradient Vanishing Problem(기울기값이 소실)는 인공신경망을 기울기값을 베이스로 하는 method (back propagation)로 학습시키려고 할 때 발생되는 어려움이다.

특히 이 문제는 네트워크에서 앞쪽 레이어의 파라미터들을 학습시키고, 튜닝하기 정말 어렵게 만든다 (Backprop은 뒤에서 앞으로 진행되므로 중간에 기울기가 소실되면 앞단 layer들의 업데이트가 안된다). 이 문제는 신경망 구조에서 레이어가 늘어날수록 더 악화된다.

이것은 뉴럴 네트워크의 근본적인 문제점이 아니다. 이것은 특정한 activation function를 통해서 기울기 베이스의 학습 method를 사용할 때 문제가 된다.

자 한번 이러한 문제를 직관적으로 이해해보고, 그것으로 인해 야기되는 문제를 짚어보자.

### Problem

Gradient 기반의 method는 parameter value의 작은 변화가 network output에 얼마나 영향을 미칠지를 이해하는 것을 기반으로 parameter value를 학습시킨다.

만약 parameter value의 변화가 network's output의 매우 작은 변화를 야기한다면, 네트워크는 parameter를 효과적으로 학습시킬 수 없게 되는데 이것이 문제다.

gradient라는 것이 결국 미분값 즉 변화량을 의미하는데, 이 변화량이 매우 작다면, network 를 효과적으로 학습시키지 못하고, error rate이 미쳐 다 낮아지지 못한채 수렴해버리는 문제가 발생한다는 것 같다.

이 상황이 바로 vanishing gradient problem으로 발생하는 문제인데, 이 문제로 초기 레이어에서 각각의 parameter들에 대한 network's output의 gradient는 극도로 작아지게 된다. 이걸 좀 근사하게 얘기하면, 초기 레이어에서 parameter value에 대해 큰 변화가 발생해도 output에 대해서 큰 영향을 주지 못한다는 것이다.

그렇다면, 언제, 왜 이러한 문제가 발생하는지 살펴보자.

### Cause

Vanishing gradient problem은 activation function을 선택하는 문제에 의존적으로 일어난다. sigmoid나, tanh 등 요즘 많이들 사용하는 activation function들은 매우 비선형적인 방식으로 그들의 input을 매우 작은 output range로 짓이겨넣는다

예를 들어서, sigmoid는 실수 범위의 수를 [0, 1]로 맵핑한다. 그 결과로 매우 넓은 input space 지역이 극도로 작은 범위로 맵핑되어버린다.

이렇게 되어 버린 input space에서는 큰 변화가 있다고 하더라도, output에는 작은 변화를 보이게 된다. gradient(기울기)가 작기 때문이다.

이러한 현상은 우리가 서로(??)의 꼭대기 층에 그러한 비선형성을 여러개 레이어로 쌓을 때 더욱 악화된다.

예를들어, 첫 레이어에서 넓은 input region을 작은 output region으로 맵핑하고, 그것이 2차 3차 레이어로 갈수록 더 심각하게 작은 region으로 맵핑되는 경우이다.

그 결과로 만약 첫 레이어 input에 대해 매우 큰 변화가 있다고 하더라도 output을 크게 변화시키지 못하게 된다.

우리는 이러한 문제를 해결하기 위해 짓이겨 넣는식('squashing')의 특징을 갖지 않는 activation function을 사용할 수 있다.

ReLU(Rectified Linear Unit - max(0, x))가 잘 선택되는 편이다.

### 참고

- https://www.quora.com/What-is-the-vanishing-gradient-problem
