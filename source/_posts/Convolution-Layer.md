---
title: Convolution Layer 구현
date: 2020-08-29 15:40:48
categories:
  - Tech
  - 딥러닝
tags: [CNN, DeepLearning]
mathjax: true
---

cs231n assignment2의 `Convolution Layer`를 구현해보며 느낀점을 정리해보았다.
개념보다는 구현에 초점을 맞춘 포스트이므로 참고바람.

### Convolution Layer

CNN의 핵심은 convolution operation 즉, 합성곱 연산이다. 기존의 Fully Connected Layer와 다른 Convolution Layer의 특징은 Input의 Spatial Structure, 공간적 구조를 보존한다는 것이다. Convolution 연산을 수행하면 input에 대한 feature map을 뽑을 수 있는데,

{% img /image/post_image/20200829/conv_layer1.png '요로코롬1'%}
{% img /image/post_image/20200829/conv_layer2.png '요로코롬2'%}

filter의 개수에 따라 output인 activation map의 depth가 달라지며, filter가 어떤식으로 input을 sliding 하는지에 따라 activation map의 size가 변화한다. input_size(N), filter_size(F), padding(p) 등의 조건이 있다면 다음의 식으로 간단하게 Output size를 알아낼 수 있다.

$$(N + 2p - F) / Stride + 1 $$

그럼 정리해보자면,
Convolution layer란 녀석은 input을 각 필터로 sliding하면서 값을 계산하기만 하면 되는 짜기 easy한 녀석이 아닐까?

맞긴한데 짜면서 아주 호되게 혼났다.

### Forward Pass

```python
def conv_forward_naive(x, w, b, conv_param):
"""
Input:
- x: Input data of shape (N, C, H, W)
- w: Filter weights of shape (F, C, HH, WW)
- b: Biases, of shape (F,)
- conv_param: A dictionary with the following keys:
	- 'stride': The number of pixels between adjacent receptive fields in the horizontal and vertical directions.
	- 'pad': The number of pixels that will be used to zero-pad the input.

Returns a tuple of:
- out: Output data, of shape (N, F, H', W') where H' and W' are given by
	H' = 1 + (H + 2 * pad - HH) / stride
	W' = 1 + (W + 2 * pad - WW) / stride
- cache: (x, w, b, conv_param)
"""

out = None
cache = (x, w, b, conv_param)

return out, cache
```

문제를 조금 해석해보자면,

1. size가 H x W x C(channel의 수, rgb 같은) 인 이미지 N개가 input x로 들어왔다.
2. input을 size가 HH x WW x C인 필터 F개로 sliding 하고,
3. size가 F인 bias도 더해서,
4. size가 H'x W'x F인 out N개로 만들어내라.
5. 이때 H', W'은 위에서 살펴보았던 식으로 잘 계산해라

요런 문제이다.
그럼 computation graph를 한번 생각해보자.

{% img /image/post_image/20200829/Conv_forward.jpeg 'computational graph'%}

다이렉트로 out이 나오는 그래프는 아니다.
먼저 input x에 padding을 추가해주고, filter의 크기만큼을 crop한 뒤, filter와 내적을 취해주고, bias term을 더해주는 과정이다.
이 과정을 거치고 나면 (N,F)의 spatial_out이 계산되는 데,

{% img /image/post_image/20200829/conv_layer3.png 'spatial_out 생김새'%}

요 그림에서 가운데 공이 담긴 녀석(공이 각각의 필터가 계산한 값이고, F개 있다고 보면 됨)이 N개 있는 꼴이라고 생각하면 된다.

이 과정을 H'xW' 번 해주면 우리가 원하는 out(N,F,H',W')을 얻어낼 수 있는 것이다!

```python
N, C, H, W = x.shape
F, C, HH, WW = w.shape

stride = conv_param['stride']
pad = conv_param['pad']

npad = ((0,0),  (0,0),  (pad,pad),  (pad,pad)) # padding 위한 값
filter_size = C*HH*WW # 내적의 용이성을 위해 미리 계산해두는 값

H_out = int(1 + (H + 2 * pad - HH) / stride) # H'
W_out = int(1 + (W + 2 * pad - WW) / stride) # W'

out = np.zeros((N, F, H_out, W_out)) # 최종적으로 구할 out 초기화 (N, F, H', W')
x_pad = np.pad(x, npad,  'constant', constant_values=(0)) # x에 pad 크기만큼 zero-padding

for height in  range(H_out):
	for width in  range(W_out):
		x_crop = x_pad[np.arange(N),  :, height*stride:height*stride+HH, width*stride:width*stride+WW]  # x에서 (N,C,HH,WW)크기만큼을 crop
		x_crop_stretch = x_crop.reshape(N, filter_size)  # (N, filter_size)
		w_stretch = w.reshape(F, filter_size)  # (F, filter_size)
		spatial_out = np.dot(x_crop_stretch, w_stretch.T) + b.reshape((1,F)) # (N,F)
		out[np.arange(N),  :, height, width] = spatial_out
```

### Backward Pass

포워드 열심히 짰으니 이젠 backward 짤 차례다.
쫄면 안된다. (이자는 쫄았다)

```python
def conv_backward_naive(dout, cache):
    """
    Inputs:
    - dout: Upstream derivatives.
    - cache: A tuple of (x, w, b, conv_param) as in conv_forward_naive

    Returns a tuple of:
    - dx: Gradient with respect to x
    - dw: Gradient with respect to w
    - db: Gradient with respect to b
    """
    dx, dw, db = None, None, None
    return dx, dw, db
```

문제는 참 심플하다.
아까 그려놓은 computational graph보면서 dx, dw, db를 구하면 된다.

```python
	x, w, b, conv_param = cache
	N, C, H, W = x.shape
	F, C, HH, WW = w.shape
	N, F, H_out, W_out = dout.shape

	stride = conv_param['stride']
	pad = conv_param['pad']

	npad = ((0,0), (0,0), (pad,pad), (pad,pad)) # x = (N, C, H+2*pad, W+2*pad)
	filter_size = C*HH*WW # scalar, for stretch
	x_pad = np.pad(x, npad, 'constant', constant_values=(0))

	dx_pad = np.zeros((N,C,H+2*pad,W+2*pad))
	dw = np.zeros((F,C,HH,WW))
	db = np.zeros(F)

	for height in range(H_out):
		for width in range(W_out):
			dspatial_out = dout[np.arange(N), :, height, width] # (N, F)
			db += np.sum(dspatial_out, axis=0) # (F, )

			x_tmp = x_pad[np.arange(N), :, height*stride:height*stride+HH, width*stride:width*stride+WW] # (N, C, HH, WW)
			dw_stretch = np.dot(dspatial_out.T, x_tmp.reshape(N, filter_size)) # (N,F).T @ (N,filter_size) = (F, filter_size)
			dw += dw_stretch.reshape(F,C,HH,WW)

			w_stretch = w.reshape(F, filter_size) # (F, filter_size)
			dx_tmp_stretch = np.dot(dspatial_out, w_stretch) # (N,F) @ (F,filter_size) = (N, filter_size)
			dx_tmp = dx_tmp_stretch.reshape(N,C,HH,WW)
			dx_pad[np.arange(N), :, height*stride:height*stride+HH, width*stride:width*stride+WW] += dx_tmp

	dx = dx_pad[np.arange(N), :, pad:H+pad, pad:W+pad] # padding 제거한 값
```

간략히 설명하자면,

spatial_out을 구해서 전체 out을 만들어 냈던 것처럼, dout에서 dspatial_out(N,F)을 crop하여 gradient를 계산해 나간다. 주의해야 할 것은 padding된 x값을 이용해 만들어 낸 out 이었기 때문에, dx_pad에 gradient를 계산해 놓고, padding을 제거한 값을 dx에 할당해야 한다는 것이다.

backward 그래프도 그렸으나, 너무 지저분해서 올리진 않겠다..
(아이패드 갖고 싶습니다...)

### 느낀점

> 4중 for문 돌릴 뻔했다.

무엇을 기준으로 sliding 해나갈지가 너무 애매해서 일단 N, F, H, W 모두 돌렸다.근데 그렇게 한번 짜고나니, N, F에 대해서는 Vectorize의 가능성이 보였고, 짰고, 돌아갔다. 짜릿하더라. 뭐 요정도 일 것 같다.
