---
title:  "딥러닝기초#1 인공신경망"
date: 2020-10-09 00:00
categories: [Education]
search: true
---
## 1. 개요
딥러닝 (Deep Learning) 은 인공신경을 단위로 층층이 쌓인 인공신경망을 다루는 분야 입니다. 이번 시간에는 인공신경을 구성하는 가중치, 편향, 활성 함수를 알아보고 인공신경의 입력과 출력을 이해합니다.

## 2. 인공신경
인공신경은 인공신경망의 기본 단위로 외부에서 입력을 받아 신호를 만들어 냅니다.  가중치 (Weight), 편향 (Bias), 활성 함수 (Activation function) 로 구성됩니다.
<center><img src="/assets/images/education/artificial_neuron.png" width="40%"></center>
<center style="font-size:15px">The Artificial Neuron</center>

##### 2.1. 가중치 (Weight)
가중치 ($w_{i}$) 는 입력 신호 ($x_{i}$) 에 곱해지는 값으로 인공신경이 받아들이는 신호의 크기를 조절합니다.

##### 2.2. 편향 (Bias)
편향 ($b$) 인공신경의 출력을 조절하는 상수입니다.

##### 2.3. 활성 함수 (Activation function)
활성 함수 ($f$) 는 인공신경의 출력을 변환시켜 인공신경망이 다양한 표현을 가지게 합니다.

## 3. 인공신경의 입력과 출력
인공신경은 입력을 받아 가중치, 편향, 활성함수에 의해 결정된 출력을 내보냅니다. 입력과 출력 사이의 관계는 다음과 같습니다.

$$
\begin{aligned}
z &= \Sigma x_{i}w_{i} + b \\
 \\
y &= f(z) \\
  &= f(\Sigma x_{i}w_{i} + b)
\end{aligned}
$$

## 4. 인공신경망
인공신경망은 인공신경들이 망 (Network)을 이룬 것을 의미합니다. 이 때 인공신경의 출력은 다른 인공신경의 입력으로 작용합니다. 인공신경망은 학습을 통해 영상, 소리, 언어 등 다양한 문제를 처리할 수 있습니다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7f/Two_layer_ann.svg/800px-Two_layer_ann.svg.png" width="40%"></center>
<center style="font-size:15px">Artificial neural network (wikipedia.org)</center><br>

다음 시간에는 인공신경망 (딥러닝) 에서 학습의 의미와 학습 방법에 대해 살펴보겠습니다.