---
title:  "딥러닝기초#4 경사하강법 연습 (코드)"
date: 2020-10-10 01:00
categories: [Education]
search: true
---
## 1. 개요
앞서 배운 경사하강법을 Python 으로 구현해 보겠습니다. 다음 코드는 [딥러닝기초#3 경사하강법 연습 (이론)](..\03_gd_exercise_theory) 의 인공신경망을 작성한 것 입니다. 코드를 통해 경사하강법이 어떻게 적용되는지 자세히 살펴보겠습니다. <a href="https://github.com/phykn/example_code/blob/main/dl_basic/mnist_gd.ipynb">[전체 코드 바로가기]</a>

## 2. 모델 초기화
```python
class NET:
    def __init__(self, lr=0.01):
        self.w = np.random.uniform(
            low=-np.sqrt(6/110), 
            high=np.sqrt(6/110), 
            size=(784, 10)
        )
        self.b = np.zeros(10)
        self.lr = lr
```
NET 이라는 이름의 클래스를 만들고 모델을 초기화 합니다. 784개 입력과 10개 출력을 연결할 가중치 $w_{ij}$ 를 `self.w` 로, 편향 $b_{j}$ 는 `self.b` 로 만들었습니다. `lr` 은 학습률입니다.

## 3. 확률 계산
```python
    def forward(self, x):
        self.z = np.dot(x, self.w) + self.b
        self.p = self.sigmoid(self.z)
        return self.p
```
`forward` 함수는 확률을 계산합니다. 입력 `x` 를 받아 가중치 `self.w` 와 편향 `self.b` 으로부터 `self.z` 를 계산하고, 활성함수를 적용해 확률 `self.p` 을 얻습니다. 활성함수인 시그모이드는 아래와 같이 구현할 수 있습니다.

```python
    def sigmoid(self, x):
        return 1/(1+np.exp(-x))
```

## 4. 경사하강법 적용
[딥러닝기초#3 경사하강법 연습 (이론)](..\03_gd_exercise_theory) 에서 얻은 경사하강법 규칙은 아래와 같습니다.

$$
w_{ij} \gets w_{ij} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j}) x_{i}, \quad b_{j} \gets b_{j} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j})
$$

`update` 함수는 경사하강법 규칙에 따라 변수를 바꿔줍니다. 
```python
    def update(self, x, t):
        p = self.forward(x)
        
        dE_dp = p - t
        dp_dz = p * (1 - p)
        
        dE_dw = np.einsum('j,i->ij', dE_dp*dp_dz, x)
        dE_db = dE_dp*dp_dz
        
        self.w -= self.lr * dE_dw
        self.b -= self.lr * dE_db        
```

- `self.forward(x)` 에서 입력을 받아 확률 계산
- 확률 `p` 와 정답 `t` 로부터 `dE_dp` $({\partial E} / {\partial p_{j}} = p_{j}-t_{j})$, `dp_dz` $({\partial p_{j}} / {\partial z_{j}} = p_{j}(1-p_{j}))$ 계산
- 기울기 `dE_dw` $({\partial E} / {\partial w_{ij}})$, `dE_db` $({\partial E} / {\partial b_{j}})$ 계산
- `-=` 연산자로 현재 가중치, 편향 업데이트

## 5. 결과
총 60,000 개 훈련 데이터로 학습시키며 오차를 계산했습니다. 데이터가 인공신경망에 입력될 수록 오차가 점점 작아져 0 부근으로 몰리는 것을 확인 할 수 있습니다. 10,000 개의 테스트 데이터로 검증한 모델의 정확도는 88.57 % 입니다.
<center><img src="/assets/images/education/graph_numpy.png"></center>
<center style="font-size:15px">학습 오차</center><br>

딥러닝 라이브러리는 복잡한 코드 없이도 경사하강법을 자동으로 적용해 줍니다. 다음 시간에는 Tensorflow를 사용해 모델을 만들고 학습해 보겠습니다.