---
title:  "딥러닝기초#3 경사하강법 연습 (이론)"
date: 2020-10-10 00:00
categories: [Education]
search: true
---
## 개요
직접 모델을 만들어 경사하강법 적용 방법을 배워보겠습니다. 먼저 배경이론을 공부하고 코드로 구현해보겠습니다. 데이터로는 MNIST (Modified National Institute of Standards and Technology database) 사용합니다. MNIST는 손으로 쓴 숫자들로 이루어진 데이터 세트 입니다. 손으로 쓴 숫자를 컴퓨터가 인식하게 하는 것이 우리의 목적입니다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/2/27/MnistExamples.png" width="40%"></center>
<center style="font-size:15px">MNIST sample image (wikipedia.org)</center><br>

데이터 출처: <a href="http://yann.lecun.com/exdb/mnist/">http://yann.lecun.com/exdb/mnist/</a>

## 모델
<center><img src="/assets/images/education/model.png" width="90%"></center>
<center style="font-size:15px">Model structure</center><br>

위 그림은 모델 구조입니다. 이 모델은 아래 4 가지 과정을 수행합니다.

##### 1. 이미지 숫자 변경
데이터는 28 픽셀 X 28 픽셀 이미지 입니다. 각 픽셀의 색깔을 0~255 숫자로 변경합니다.

##### 2. 차원 변경
2차원 이미지 (28x28) 를 1차원 벡터 (784) 로 변경합니다. 차원 변경된 값 $x_{i}$ 는 인공신경망의 입력 값입니다.

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

$w_{ij}$ 는 $x_{i}$ 를 $z_{j}$ 로 연결하는 가중치, $b_{j}$ 는 편향을 의미합니다.

##### 4. 활성 함수 적용
$z$ 에 활성함수를 적용해 0~1 사이의 확률 $p$ 를 만듭니다. $p_{i}$ 는 숫자가 ($i$) 일 확률이 됩니다. 예를 들어 $p_{0}$ 은 숫자가 0일 확률, $p_{9}$ 은 숫자가 9일 확률 입니다. 활성함수로는 시그모이드 함수를 사용합니다. 시그모이드는 $-\infty \sim \infty$ 범위를 갖는 입력값이 $0 \sim 1$ 사이에 오도록 합니다. 시그모이드 함수는 아래와 같습니다.

$$
p_{j} = \frac{1}{1+e^{-z_{j}}}
$$

## 배경이론
##### 1. 경사하강법
오차($E$) 를 아래와 같이 가정해 보겠습니다. $t_{j}$ 는 정답확률, $p_{j}$ 는 예측 확률입니다.

$$
E = \frac{1}{2}\sum(t_{j}-p_{j})^{2}
$$

예를 들어 정답 숫자가 9 이고 $p$ 가 $[0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.9]$ 이면 $E$는 다음과 같이 계산됩니다.

$$
\begin{aligned}
E &= \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} \\
&+ \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(0-0.1)^{2} + \frac{1}{2}(1-0.9)^{2} \\
&= 10\times\frac{1}{2}(0.1)^{2} \\
&= 0.05
\end{aligned}
$$

오차 $E$ 에 영향을 주는 변수는 가중치 $w_{ij}$ 와 편향 $b_{j}$ 입니다. 아래와 같이 가중치와 편향에 경사하강법을 적용하면 모델을 학습시킬 수 있습니다. 

$$
w_{ij} = w_{ij} - \alpha \frac{\partial E}{\partial w_{ij}}, \quad b_{j} = b_{j} - \alpha \frac{\partial E}{\partial b_{j}}
$$

##### 2. 기울기 구하기
$E$ 는 합성함수이므로 합성함수의  미분을 활용해 기울기를 계산합니다.

$$
\frac{\partial E}{\partial w_{ij}} = \frac{\partial E}{\partial p_{j}} \frac{\partial p_{j}}{\partial z_{j}} \frac{\partial z_{j}}{\partial w_{ij}}, \quad
\frac{\partial E}{\partial b_{j}} = \frac{\partial E}{\partial p_{j}} \frac{\partial p_{j}}{\partial z_{j}} \frac{\partial z_{j}}{\partial b_{j}}
$$

이로써 오차에 대한 가중치와 편향의 미분은 세 가지 항의 곱이 되었습니다. 각 항을 개별로 계산해 보겠습니다.

##### 2.1 오차
$w_{ij}$ 는 $j$ 항에만 영향을 주므로 $j$ 번째 오차만 고려합니다.

$$
\begin{aligned}
\frac{\partial E}{\partial p_{j}} &= \frac{\partial}{\partial p_{j}}\frac{1}{2}\sum(t_{j}-p_{j})^{2} \\
&= p_{j} - t_{j}
\end{aligned}
$$

##### 2.2 활성 함수
${\partial p_{j}} / {\partial z_{j}}$ 는 활성함수인 시그모이드 함수의 미분입니다. 

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
${\partial z_{j}} / {\partial w_{ij}}$, ${\partial z_{j}} / {\partial b_{j}}$ 는 다음과 같습니다.

$$
\begin{aligned}
\frac{\partial z_{j}}{\partial w_{ij}} &= \frac{1}{\partial w_{ij}} \left( \sum x_{i}w_{ij} + b_{j} \right) = x_{i} \\
\frac{\partial z_{j}}{\partial b_{j}} &= \frac{1}{\partial b_{j}} \left( \sum x_{i}w_{ij} + b_{j} \right) = 1
\end{aligned}
$$

##### 2.4 결과
위 결과를 모두 정리하면 아래와 같은 변수 변경 규칙을 얻을 수 있습니다.

$$
w_{ij} \gets w_{ij} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j}) x_{i}, \quad b_{j} \gets b_{j} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j})
$$