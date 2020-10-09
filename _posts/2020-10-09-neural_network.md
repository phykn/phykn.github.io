---
title:  "딥러닝기초#1 인공신경망"
date: 2020-10-09
categories: [Education]
search: true
---
## 인공신경
인공신경은 외부 입력을 받아 신호를 만들어 내는 단위입니다.  가중치 (Weight), 편향 (Bias), 활성 함수 (Activation function) 로 구성됩니다.
<center><img src="/assets/images/artificial_neuron.png" width="40%"></center>
<center style="font-size:15px">The Artificial Neuron</center>

### 가중치 (Weight)
가중치 $w_{i}$ 는 입력 신호 $x_{i}$ 의 세기를 바꿉니다. 인공신경은 가중치를 통해 입력 신호 크기를 조절 할 수 있습니다.

### 편향 (Bias)
편향 $b$ 는 인공신경의 상수입니다. 편향은 인공신경의 출력을 조절합니다.

### 활성 함수 (Activation function)
활성 함수 $f$ 는 인공신경에 비선형성을 부여합니다. 이는 인공신경망이 다양한 표현을 가지게 합니다.

### 입력과 출력
입력은 인공신경의 입력 신호를 의미합니다. 인공신경은 입력 $x$ 를 받아 출력 $y$ 를 내보냅니다. 입력과 출력 사이의 관계는 다음과 같습니다.

$$
\begin{aligned}
z &= \Sigma x_{i}w_{i} + b \\
 \\
y &= f(z) \\
  &= f(\Sigma x_{i}w_{i} + b)
\end{aligned}
$$

## 인공신경망
인공신경망은 인공신경들이 망 (Network)을 이룬 것을 의미합니다. 인공신경의 출력은 다른 인공신경의 입력으로 작용합니다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7f/Two_layer_ann.svg/800px-Two_layer_ann.svg.png" width="40%"></center>
<center style="font-size:15px">Artificial neural network (wikipedia.org)</center>