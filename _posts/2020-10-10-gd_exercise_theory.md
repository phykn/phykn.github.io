---
title:  "딥러닝기초#3 경사하강법 연습 (이론)"
date: 2020-10-10 00:00
categories: [Education]
search: true
---
## 개요
경사하강법을 사용해 간단한 모델을 학습시켜보겠습니다.

## 데이터
MNIST (Modified National Institute of Standards and Technology database) 를 사용합니다. MNIST는 손으로 쓴 숫자들로 이루어진 데이터 세트 입니다. 손으로 쓴 숫자를 컴퓨터가 인식하게 하는 것이 목적입니다.
<center><img src="https://upload.wikimedia.org/wikipedia/commons/2/27/MnistExamples.png" width="40%"></center>
<center style="font-size:15px">MNIST sample image (wikipedia.org)</center><br>

데이터 출처: <a href="http://yann.lecun.com/exdb/mnist/">http://yann.lecun.com/exdb/mnist/</a>

## 모델
<center><img src="/assets/images/education/model.png" width="90%"></center>
<center style="font-size:15px">Model structure</center><br>

위 그림은 모델 구조를 나타냅니다. 이 모델은 아래 4 가지 과정을 수행합니다.

##### 1. 이미지 숫자 변경
데이터는 28 픽셀 X 28 픽셀 이미지 입니다. 각 픽셀의 색깔을 0~255 숫자로 변경합니다.

##### 2. 차원 변경
2차원 이미지 (28x28) 를 1차원 벡터 (784) 로 변경합니다. 차원 변경된 값 $x_{i}$ 는 모델의 입력 값입니다.

##### 3. 길이 축소
인공신경망을 사용해 길이 784 벡터를 길이 10 벡터로 축소 시킵니다. $z$와 $x$ 사이의 관계는 다음과 같습니다.

$$
\begin{aligned}
z_0 &= \sum x_{i}w_{i0} + b_{0} \\
z_1 &= \sum x_{i}w_{i1} + b_{1} \\
z_2 &= \sum x_{i}w_{i2} + b_{2} \\
   & ... \\
z_j &= \sum x_{i}w_{ij} + b_{j}
\end{aligned}
$$

이 때, $w_{ij}$ 는 $x_{i}$ 를 $z_{j}$ 로 연결하는 가중치, $b_{j}$ 는 $z_{j}$ 의 편향입니다.

##### 4. 활성 함수 적용
$z$ 에 활성함수를 적용해 확률 $p$ 를 만듭니다. 예를 들어 $p_{0}$ 은 숫자가 0일 확률, $p_{9}$ 은 숫자가 9일 확률 입니다. 여기서는 활성함수로 시그모이드 함수를 사용합니다. 시그모이드는 $-\infty \sim \infty$ 범위를 갖는 입력값이 $0 \sim 1$ 사이에 오도록 합니다.

$$
p_{j} = \frac{1}{1+e^{-z_{j}}}
$$

## 이론
##### 1. 오차와 경사하강법
오차($E$) 를 아래와 같이 가정해 보겠습니다. $t_{j}$ 는 정답확률, $p_{j}$ 는 예측 확률입니다.

$$
E = \frac{1}{2}\sum(t_{j}-p_{j})^{2}
$$

예를 들어 정답 숫자가 9 이고 $p$ 가 $[0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.9]$ 이면 $E$는 다음과 같습니다.

$$
\begin{aligned}
E &= \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} \\
&+ \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(1-0.9)^{2} \\
&= 10\times\frac{1}{2}(0.1)^{2} \\
&= 0.05
\end{aligned}
$$

오차 $E$ 에 영향을 주는 변수는 가중치 $w_{ij}$ 와 편향 $b_{j}$ 입니다. 따라서 가중치와 편향에 경사하강법을 적용해 모델을 학습시킬 수 있습니다. 

$$
w_{ij} = w_{ij} - \alpha \frac{\partial E}{\partial w_{ij}}, \quad b_{j} = b_{j} - \alpha \frac{\partial E}{\partial b_{j}}
$$

##### 2. 기울기 구하기
미분의 연쇄법칙을 사용해 다음과 같이 기울기를 계산할 수 있습니다.

$$
\frac{\partial E}{\partial w_{ij}} = \frac{\partial E}{\partial p_{j}} \frac{\partial p_{j}}{\partial z_{j}} \frac{\partial z_{j}}{\partial w_{ij}}, \quad
\frac{\partial E}{\partial b_{j}} = \frac{\partial E}{\partial p_{j}} \frac{\partial p_{j}}{\partial z_{j}} \frac{\partial z_{j}}{\partial b_{j}}
$$

##### 2.1 오차
$w_{ij}$ 는 $j$ 항에만 영향을 주므로 $j$ 번째 오차만 고려합니다.

$$
\begin{aligned}
\frac{\partial E}{\partial p_{j}} &= \frac{\partial}{\partial p_{j}}\frac{1}{2}\sum(t_{j}-p_{j})^{2} \\
&= p_{j} - t_{j}
\end{aligned}
$$

##### 2.2 활성 함수
${\partial p_{j}} / {\partial z_{j}}$ 는 활성함수의 미분입니다. 시그모이드의 미분은 다음과 같습니다.

$$
\begin{aligned}
\frac{\partial p_{j}}{\partial z_{j}} &= \frac{\partial}{\partial z_{j}}\frac{1}{1+e^{-z_{j}}} \\
&= (-1) \times \left( \frac{1}{1+e^{-z_{j}}} \right)^{2} \times \frac{\partial}{\partial z_{j}} (1+e^{-z_{j}}) \\
&= (-1) \times \left( \frac{1}{1+e^{-z_{j}}} \right)^{2} \times (-1) \times e^{-z_{j}} \\ 
&= \frac{1}{1+e^{-z_{j}}} \times \frac{e^{-z_{j}}}{1+e^{-z_{j}}} \\
&= p_{j} \frac{e^{-z_{j}}}{1+e^{-z_{j}}} \\
&= p_{j} \left( 1- \frac{1}{1+e^{-z_{j}}} \right) \\
&= p_{j}(1- p_{j})
\end{aligned}
$$

##### 2.3 가중치, 편향
$w_{ij}$ 와 $b_{j}$ 에 대한 미분은 다음과 같습니다.

$$
\begin{aligned}
\frac{\partial z_{j}}{\partial w_{ij}} &= \frac{1}{\partial w_{ij}} \left( \sum x_{i}w_{ij} + b_{j} \right) = x_{i} \\
\frac{\partial z_{j}}{\partial b_{j}} &= \frac{1}{\partial b_{j}} \left( \sum x_{i}w_{ij} + b_{j} \right) = 1
\end{aligned}
$$

##### 2.4 결과
정리하면 변수 $w_{ij}$, $b_{j}$ 의 경사하강법 규칙은 아래와 같습니다.

$$
w_{ij} = w_{ij} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j}) x_{i}, \quad b_{j} = b_{j} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j})
$$