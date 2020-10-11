---
title:  "딥러닝기초#1 인공신경망"
date: 2020-10-09 00:00
categories: [Education]
search: true
---
## 인공신경
인공신경은 외부 입력을 받아 신호를 만들어 내는 단위로  가중치 (Weight), 편향 (Bias), 활성 함수 (Activation function) 로 구성됩니다.
<center><img src="/assets/images/education/artificial_neuron.png" width="40%"></center>
<center style="font-size:15px">The Artificial Neuron</center>

##### 가중치 (Weight)
가중치 ($w_{i}$) 는 입력 신호 ($x_{i}$) 의 세기를 바꿉니다. 인공신경은 가중치를 통해 입력 신호 크기를 조절합니다.

##### 편향 (Bias)
편향 ($b$) 는 상수로 인공신경의 출력을 조절합니다.

##### 활성 함수 (Activation function)
활성 함수 ($f$) 는 인공신경에 비선형성을 부여해 다양한 표현을 가지게 합니다.

## 인공신경의 입력과 출력
인공신경은 입력을 받아 가중치, 편향, 활성함수에 의해 결정된 출력을 내보냅니다. 입력과 출력 사이의 관계는 다음과 같습니다.

$$
\begin{aligned}
z &= \Sigma x_{i}w_{i} + b \\
 \\
y &= f(z) \\
  &= f(\Sigma x_{i}w_{i} + b)
\end{aligned}
$$

## 인공신경망
인공신경망은 인공신경들이 망 (Network)을 이룬 것을 의미합니다. 인공신경의 출력은 다른 인공신경의 입력으로 작용합니다. 인공신경망은 학습을 통해 영상, 소리, 언어 등 다양한 문제를 처리할 수 있습니다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7f/Two_layer_ann.svg/800px-Two_layer_ann.svg.png" width="40%"></center>
<center style="font-size:15px">Artificial neural network (wikipedia.org)</center>