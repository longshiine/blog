---
title: StarGAN-v2 논문 리뷰
date: 2021-07-19 14:26:32
categories:
  - Tech
  - 딥러닝
tags: [DeepLearning, ComputerVision, GAN]
mathjax: true
---

# StarGAN-v2

**Github:** [https://github.com/clovaai/stargan-v2](https://github.com/clovaai/stargan-v2)

**Paper:** [https://arxiv.org/abs/1912.01865](https://arxiv.org/abs/1912.01865)

[Yunjey Choi](https://arxiv.org/search/cs?searchtype=author&query=Choi%2C+Y), [Youngjung Uh](https://arxiv.org/search/cs?searchtype=author&query=Uh%2C+Y), [Jaejun Yoo](https://arxiv.org/search/cs?searchtype=author&query=Yoo%2C+J), [Jung-Woo Ha](https://arxiv.org/search/cs?searchtype=author&query=Ha%2C+J)

[StarGAN-v2.pdf](/image/post_image/20210719/StarGAN-v2.pdf)

> A good image-to-image translation model should learn a mapping between different visual domains while satisfying the following properties: 1) diversity of generated images and 2) scalability over multiple domains. Existing methods address either of the issues, having limited diversity or multiple models for all domains. We propose StarGAN v2, a single framework that tackles both and shows significantly improved results over the baselines. Experiments on CelebA-HQ and a new animal faces dataset (AFHQ) validate our superiority in terms of visual quality, diversity, and scalability. To better assess image-to-image translation models, we release AFHQ, high-quality animal faces with large inter- and intra-domain differences. The code, pretrained models, and dataset can be found at this https URL.

{% img /image/post_image/20210719/intro.png '' '' 'width:100%' %}

## Abstract

- 좋은 image-to-image translation model은 다음의 특성을 만족시킨다.

  1. 생성된 이미지의 다양성

  2. 다양한 도메인으로의 확장성

- StarGAN-v2는 Single Framework로 이를 만족시키는데, CelebA-HQ와 AFHQ dataset을 통한 실험은이 모델이 **visual quality, diversity, scalability** 에 있어 superiority를 가진다는 사실을 validate 하게 한다.

## 1. Introduction

### 용어정리

- **Domain**: set of images that can be grouped as a visually distinctive category.
- **Style**: each image has a unique appearance
  - ex) 이미지의 **domain**을 사람의 성별로 설정할 수 있음. 이때 **style**은 \*\*\*\*makeup, beard(턱수염), hairstyle 등을 포함한다. → 위 그림의 celebA-HG 의 예시!
- 이상적인 i2i 변환 방법은 각 도메인의 서로 다른 스타일을 고려하여 합성할 수 있어야 한다.

  —> 하지만 너무 많은 임의의 스타일과 도메인이 데이터 셋에 있기 때문에 그런 모델을 만드는 것이 어렵다..

### 잠시 살펴보고 가는 **쉽게 쓰여진** **GAN**

[https://dreamgonfly.github.io/blog/gan-explained/](https://dreamgonfly.github.io/blog/gan-explained/)

### 기존의 접근 방식들(methods)

- 다른 스타일에 접근하기 위해, 많은 방법들은 low-dimentional latent code를 generator에 inject한다. —> uniform or Gaussian distribution에서 랜덤하게 추출된 (100, ) 짜리 벡터 등
- 그러한 방법들의 domain-specific 디코더는 이러한 latent-vector를 이미지를 생성하는 Generator의 레시피로 사용한다.
- **그러나,** 이러한 method들은 2 domain간의 mapping만을 고려한다.

  —> **# of domain에 있어 not scalable**

  —> K 도메인을 학습하기 위해선 K(K-1)개의 Generator를 학습시켜야함

  ⇒ 결국 practical usage가 떨어짐.

### StarGAN은? 머가 달라?

- Scalability에 접근하기 위해, unified framed work들이 제안되었는데, starGAN은 초기의 모델중 하나!

  —> Single generator를 사용하여 모든 가능한 domain의 mapping을 학습가능

- Generator는 domain label을 추가적인 input으로 받아 상응하는 도메인으로 변환하는 것을 배움.
- 그러나, 여전히 StarGAN은 각 도메인의 deterministic mapping만을 배울 뿐, data distribution의 multi-modal적인 특성은 포착하지 못함.

  —> **Multi-modal**: 우리의 경험은 실제로 **복합적(multimodal)**인데, 보고, 듣고, 촉감을 느끼고, 향기를 맡고, 맛을 음미하는 것이 그것이다. Modality는 어떤 일이 일어나거나 우리가 무언가를 경험하는 다양한 방식을 말한다. 그리고 이것을 활용하기 위해서는 멀티모달로 특징화 해야한다.

  —> **Multi-modal Data:** 서로 다른 형태의 정보로 이루어져 뚜렷한 특성이 구분되는 데이터

- 이러한 한계는 각 도메인이 이미 결정된 라벨로부터 결정되는 데에서 온다.
- Generator가 one-hot vector같은 fixed label을 input으로 받는 것을 주목하자. 그렇기에 필연적으로 각 도메인에 대해 같은 output을 낼 수 밖에 없다.

### StarGAN-v2: 문제에 대한 해법

- StarGAN에서 부터 출발하여(scalability에 대한 해법), 그것의 domain label을 새롭게 고안한 domain-specific style code로 변경함(multi-modal nature 파악에 대한 해법)

  —> style code는 specific한 domain의 구분되는 styles를 표현가능!

- 이것을 위해 두가지 module을 도입했는데
  1. **Mapping Network:** 랜덤 Gaussian Noise를 style code로 transform 하는 녀석
  2. **Style Encoder:** 주어진 reference이미지에서 style code를 추출하는 녀석 \*\*\*\*
- 이렇게 생성된 style codes를 가지고, Generator는 성공적으로 여러 도메인의 서로 다른 이미지를 합성해내는 것을 배우게 된다!

- 논문은 우선 StarGAN-v2의 individual component들의 효과를 연구했고, **style code를 활용하는 것이 실제로 효과가 있음**을 보인다. (Section 3.1 에서)
- 그리고 제안한 방법(StarGAN-v2)이 multiple domain에 대해 scalable하며, visual quality와 diversity에 있어서도 이전의 방법들보다 나음을 확인했다.

## 2. StarGAN v2

### 2.1 Proposed framework

- X, Y를 sets of images and possible domain이라고 하자.

  → X = sets of images, Y = sets of possible domain

- 이미지 x와, 임의의 도메인 y에 대해 StarGAN-v2의 목적은 **single generator G**가 x에 상응하는 도메인 y에 대한 구분 가능한 이미지를 생성하게 하는 것이다.

  —> 어떤 이미지 x가 있고, "금발" 이라는 도메인이 있을때 **G**가 x의 다양한 금발 버전을 만들어 낼 수 있게 하는 것

  —> 근데 이때 아마 위에서 얘기한 multi-modal 적인 data의 특성을 반영하기 위함의 관점에서도 style code를 domain label대신 쓰지 않을까(여성이고, 금발이며, 서양인이고 ~~ 등의 복합적인 도메인으로의 변환)

- 우리는 사전에 학습된 각 도메인의 style space 내에서 domain-specific style vectors를 생성해 내고, **G**로 하여금 style vector를 반영하도록 학습시킨다.
- 아래의 Figure 2가 전체 프레임워크의 overview를 보여준다. 총 4개의 모듈로 구성된다.

  {% img /image/post_image/20210719/1.png '' '' 'width:100%' %}

#### **(a)** **Generator**

- generator **G**는 input 이미지 x를 output image G(x,s)로 변환하는데 이때, mapping network **F** 혹은 style encoder **E** 중 하나를 통해 생성된 domain-specific style code **s**를 반영한다.
- **AdaIN**(Adaptive instance normalization)을 사용한다. —> to inject **s** into **G**
- 이 "**s**"가 style of specific domain y를 표현한다는 것을 발견했고, 이에 y(domain 정보)를 G에게 주지 않기 때문에 —> **G가 all domain의 image들을 synthesis할 수 있게 되었다!!!**

  —> domain label 대신 style code를 Generator에 input으로 줌으로서 scalability를 챙길 수 있었다!

#### **(b) Mapping network**

- 주어진 latent(잠재) code z와 domain y에 대하여, mapping network F는 style code $s = F_y(z)$ 를 생성한다.
- **F**는 모든 가능한 도메인에 대한 style codes를 제공하기 위해 여러 output branch를 가진 MLP로 구성된다.
- F는 diverse style codes 를 $z \in Z$ 와 $y \in Y$ 에서 randomly하게 sampling 하여 생성할 수 있다.
- 논문에서 보이는 multi-task architecture는 F로 하여금 효율적이고 효과적으로 모든 domain에 대한style representations 을 학습하게 한다.

#### **(c) Style encoder**

- 주어진 image x와 상응하는 domain y에 대하여 encoder E는 style code $s = E_y(s)$를 추출한다.
- F와 비슷하게, E도 multi-task learning setup에서 이점이 있다. E는 different reference 이미지를 사용하여 diverse style codes를 만들어낸다.
- 이것은 G로 하여금 reference 이미지 x의 style s를 반영하는 output image를 합성해 낼 수 있게 한다!

#### **(d) Discriminator**

- D는 multiple output branches로 구성되는 multi-task discriminator이다.
- 각각의 branch $D_y$는 이미지 x가 domain y 에 해당하는 진짜 이미지인지, fake이미지 $G(x,s)$인지를 구분하는 binary classification을 학습한다.

### 2.2 Training Objectives

주어진 이미지 $x \in X$, 그에 대한 original domain $y \in Y$ 에 대해 다음의 목적함수(objectives)를 통해 학습한다.

#### **Adversarial objective**

- training 시에, 우리는 latent code $z \in Z$와 target domain $\tilde{y} \in Y$ 을 randomly sample하고, target style code $\tilde{s} = F_{\tilde{y}}(z)$ 를 생성한다.
- Generator G는 이미지 x와 $\tilde{s}$를 input으로 받아 output image인 $G(x, \tilde{s})$를 생성하는 방법을 **adversarial loss를** 통해 학습한다

  $$L_{adv} = E_{x,y} [log D_{y}(x)] + E_{x, \tilde{y}, z}[log(1 - D_{\tilde{y}}(G(x,\tilde{z})))]$$

- $D_{y}(.)$는 도메인 y에 상응하는 Discriminator의 output
- mapping network F는 target domain $\tilde{y}$에 포함될 것 같은 style code  
  $\tilde{s}$ 를 생성하도록 학습이 되고, $G$는 $\tilde{s}$를 활용하여 domain $\tilde{y}$의 real image와 indistinguishable(구분불가)한 이미지 $G(x,\tilde{s})$를 생성함

#### **Style reconstruction**

- G는 style code $\tilde{s}$를 활용하게 하기 위해 style reconstruction loss를 사용함

  $$L_{sty} = E_{x, \tilde{y},z} [\parallel \tilde{s} - E_{\tilde{y}}(G(x,\tilde{s}))\parallel_{1}]$$

- 이 objective는 여러 인코더를 사용하여 image → latent code mapping을 학습하는 이전의 접근 방식들과 유사하다.
- 주목할만한 차이점은 StarGAN-v2에서는 multiple 도메인에 대한 output을 위해 **single** Encoder E에 대해서만 학습한다는 것!

  —> 이는 앞서 style code 도입을 통해 얻을 수 있는 이점과 이어진다.

- **At test time,** 학습된 encoder E는 G가 input이미지를 reference image의 style을 반영하여 transform할 수 있게 만든다!

#### **Style diversification**

- Generator G가 더욱 diverse images를 만들어 낼 수 있게 하기위해, diverse sensitive loss를 통해 명시적으로 regularize(정규화)한다.

$$L_{ds} = E_{x, \tilde{y}, z_1, z_2 } [\parallel G(x, \tilde{s}_1) - G(x, \tilde{s}_2)\parallel_1]$$

- F가 생성하는 target style codes $\tilde{s}_1$ 와 $\tilde{s}_2$ 는 $\tilde{S}_i = F_\tilde{y}(z_i)$ for $i \in \{1,2 \}$ 에서 비롯된다.
- regularization term을 최대화 하는 것은 Generator가 diverse images를 생성하기 위해 image 공간을 explore하고 의미있는 style feature를 찾게한다.
- 주의해야할 것은 기존의 form에서 denominator(분모)의 $\parallel z_1 - z_2 \parallel_1$ 가 조금의 차이만 발생해도, loss를 크게 increase하는데, 이는 training을 불안정하게 만듬
- 때문에 논문에서는 denominator part를 제거하고, 안정적인 training을 위한 새로운 equation을 고안함 —> but same intuition

#### **Preserving source characteristics**

- 생성된 이미지 $G(x,\tilde{s})$가 domain invariant characteristics(eg. pose)를 적절히 보존하도록 보장하기 위해 cycle consistency loss를 고안

$$L_{cyc}= E_{x,y,\tilde{y}, z}[\parallel x - G(G(x, \tilde{s}), \hat{s})\parallel_1]$$

- $\hat{s} = E_y(x)$ 는 input 이미지 x의 추정된 style code이고, y는 x의 original domain 이다.
- G가 input 이미지 x를 reconstruct 할때 estimated style code of input x $\hat s$를 포함시켜 줌으로서 G는 x의 style을 faithfully(충실히) 변화시키면서도 x의 원래 특성을 보존시킬 수 있다.

  **—> source의 얼굴이 그대로인 이유는 $\hat s$ 때문이구먼!**

#### **Full objective**

$$min_{G,F,E} \ \ max_D \; L_{adv} + \lambda_{sty} L_{sty} - \lambda_{ds}L_{ds} + \lambda_{cyc}L_{cyc} $$

- $\lambda$ term들은 해당 loss의 hyper-parameters
- **중요! 이 논문에서는 latent-vector(mapping network) 대신 reference image(style encoder)를 사용하여 style code를 generate할 때에도 같은 objective를 사용하였다! —> more training detail은 Appendix B 참고!**

—> 사실상 위의 $L_{sty}$ 에 대한 얘기인데, 이 objective는 latent vector를 style code로 매핑하는 mapping network F 라는 녀석에 대한 loss였는데,

—> 이게 ref image를 style code로 변환하는 style Encoder E 에도 똑같이 적용된다는 소리일까..?

## 3. Experiments

- StarGAN-v2의 components들(각각의 모듈들)을 실험했고, 3가지의 leading baseline들과 비교했다.
- 모든 실험은 unseen data에 대해서 이루어졌음

#### **Baselines**

- MUNIT, DRIT, MSGAN 을 baseline으로 삼았고, 이들은 **two domain간의 multi-modal mapping**을 수행함.
- Multi-domain에 대한 비교를 위해 이들을 각각의 이미지 domain pairs에 대해 여러번 학습시킴
- StarGAN이랑도 비교했는데(v1), 이 모델은 multiple domain에 대한 mapping을 single generator로 배움

#### **Datasets**

- CelebA-HQ와 AFHQ data set에 대해 평가
- CelebA-HQ 데이터 셋을 male, female의 두가지 도메인으로 나누었음.
- domain-label을 제외하고는 추가적인 정보(e.g. facial attributes of CelebA)를 사용하지 않았으며, 모델로 하여금 비지도적으로 style들에 대한 정보를 배우도록 했음.
- 공정한 비교를 위해 훈련시 이미지를 256X256 으로 resize했고, 이 사이지는 baseline들의 최대 resolution 사이즈임.

#### **Evaluation metrics**

- 우선 평가해야 할 metric은 두 가지인데,

      1. visual quality  —> **FID**(Frechet Inception distance): distance between two distributions of real and generated images (lower is better)

  **Q. FID가 visual quality에 대한 지표도 포함하는가..?**

  {% img /image/post_image/20210719/2.png '' '' 'width:100%' %}

  1. diversity of generated images —> **LPIPS**(Learned Perceptual Image Patch Similarity): (higher lis better)

     {% img /image/post_image/20210719/9.png '' '' 'width:80%' %}

- 위 표는 StarGAN-v1에 v2의 모듈 혹은 기법들을 한 개씩 추가하며 measure한 것!

  {% img /image/post_image/20210719/10.png '' '' 'width:80%' %}

- (A): StarGAN-v1의 결과 —> 메이크업만을 옮겨 local change만 이끌어낼 뿐임
- (B): ACGAN Discrimintor를 multi-task discriminator로 변경 —> generator가 input이미지의 global structure를 변경할 수 있게함.
- (C): R1 regularization & AdaIN 적용 —> training stablility를 증가시킴

  —> (A),(B),(C)의 경우 주어진 input image와 target domain에 대해서 multiple output을 낼수 없기 때문에, LPIPS 측정이 불가하다. ⇒ 1장에서 다양성을 measure할 수 없으므로

- (D): diversity(다양성)을 이끌어내기위해 latent code z를 generator G에 바로 넣는다는 생각을 할 수 있음 —> 실험해본 결과, multi-domain 시나리오에서 이러한 baseline는 네트워크로 하여금 의미있는 styles를 배우게 하지 못하고, 기대하는 diversity를 내지 못했다.

- **때문에**, 논문에서는

  1. **latent code가 domain을 구분 하는데에 능력이 없고,**
  2. **latent reconstruction loss는 domain-specific 한 style 보다 domain-shared style을 모델링할 것이라 추측했다.**

- G에 latent code를 direct로 주기보단, mapping network F를 통해 z에서 domain-specific style code s를 생성하여 G에 inject함
- style reconstruction loss 도 소개함
- 주목해야할 것은, mapping network의 각각의 output branch가 특정한 domian 에 대한 것이라는 것이다. 때문에 stylecode는 domain을 구분하는 것에 대한 ambiguity(모호성)이 없다.
- latent reconstruction loss(F에 대한 Loss)와 다르게, style reconstruction loss는(E에 대한 Loss) generator로 하여금, domain-specific style을 반영하여 이미지를 생성하게 한다. —> (reference image의 style code를 사용하여 이미지를 생성한다는 뜻인듯)

### 3.2 Comparison on diverse image synthesis

- 두가지 관점에서 비교할것
  1. latent-guided synthesis —> baseline들과 비교할 때, 유일하게 안깨짐 암튼 base들보다 좋다.
  2. reference-guided synthesis —> ref image로 부터 추출된 style code를 사용함으로 distinctive style을 가져올 수 있었으며, color distribution만 추출해내는 기존 기법들과 차이를 보였다.
- Human-evaluation
  - 사람들에게 투표시킬 때도 baseline 보다 나았다.

## 4. Discussion

### **First**

multi-head **mapping network, style encoder** 에 의해서 style code가 도메인에 대해 각각 생성된다.

—> Generator가 stylecode를 사용하는 것에만 초점을 맞출 수 있다.

### **Second**

기존 baseline들은 style space를 fixed Gaussian distribution을 따른다는 가정을 했으나,

StarGAN-v2에서는 StyleGAN의 insight에 따라, transformation에 대해 배운 style space에서 생성해냄

{% img /image/post_image/20210719/3.png '' '' 'width:80%' %}

- FFHQ에 대해서도 Reference-guided synthesis가 좋은 결과를 냈다.

## 5. Related work

- StyleGAN: non-linear mapping function을 도입하여, imput latent code를 style space로 매핑함.

  —> 그러나 styleGAN은 image를 input으로 받지않아서, real image를 transform 한다라는 task가 자명하지 않다.

- **본 논문의 latent-guided synthesis와 reference-guided synthesis 모두 coarsely(거칠게) labeled dataset으로 train 되었다.**

```jsx
결국 reference guided synthesis로 갈텐데,
이때 우리는 스타일 합성을 하는 것이지, 헤어 합성이 아니다.
—> 초기에는 스타일 합성 기능을 제공하는 것으로,,?
—> 대신 이를 위해 동양인 dataset에 대한 fine tuning이 필요함.
-> celebA-HQ 30000장, 1024x1024

1. 초기 launching시에 우리가 제공하는 스타일들을 보여주고,
10개정도 scrap할 수 있게(왓챠나 넷플릭스 처럼)함.

2. 우리는 AI 스타일 합성을 제공하며, 이는 해당 사진의 헤어, 메이크업 등을 포함함
3. 따라서 우리는 스타일 전반에 대한 설명 또한 제공(메이크업 정보, 헤어 정보 등)
```

## 6. Conclusion

- StarGAN-v2는 one domain to diverse images of a target domain and supporting multiple target domain

## Appendix

### B. Training details

- batch size: 8
- iteration: 100,000(100K)
- Tesla V100 GPU 1개, 3 days
- $\lambda_{sty} = 1, \; \lambda_{ds} = 1, \; \lambda_{cyc} = 1$ for CelebA-HQ
- non-saturating adversarial loss + R1 regularization $\gamma = 1$
- Adam optimizer with $\beta_1 = 0, \: \beta_2 = 0.99$
- Learning rates for $G, D, and \; E = 10^{-4}$
- Learning rates for $F = 10^{-6}$
- initialize the weights = He initialization
- set biases to zero —> except for biases associated with scaling vector of AdaIN(set to one)

### E. Network architecture

#### **Generator**

{% img /image/post_image/20210719/4.png '' '' 'width:60%' %}

#### **Mapping Network**

{% img /image/post_image/20210719/5.png '' '' 'width:60%' %}

—> K output branches가 있는데, 이는 domain의 개수를 뜻함.

—> 첫 네 개의 FC는 domain간 공유되고, 뒤의 네 개는 domain별로 존재.

—> 각각의 dimention 정보

- **latent code: 16**
- **hidden layer: 512**
- **style code: 64**

—> latent code는 standard Gaussian distribution에서 sample

—> pixel normalization을 latent code에 적용하지 않았다.(model performance를 올리지 못하는 것으로 밝혀짐)

—> feture normalization도 시도해봤으나, 이도 잘 안되는 것으로 파악

#### **Style encoder**

{% img /image/post_image/20210719/6.png '' '' 'width:60%' %}

—> CNN with K output branch

—> 6개의 pre-activation residual block이 domain간에 공유

—> Dimension 정보

- **Output: 64 x K**

#### **Discriminator**

- multi task discriminator로서 위의 테이블 공유
- K개의 FC, for real/fake classification of each domain

  —> Dimension 정보

  - **D = 1 (real/fake)**

- multi-task discriminator가 다른 conditional discriminators 보다 나음을 확인

# Fine Tuning 관련

- Azure V100 x 8개

  {% img /image/post_image/20210719/7.png '' '' 'width:100%' %}

  —> 30,000장 학습, 1개, 3일(72시간) ⇒ 8개, 9시간 2725 x 9 = **24,000원**

- V100 else

  {% img /image/post_image/20210719/8.png '' '' 'width:100%' %}
