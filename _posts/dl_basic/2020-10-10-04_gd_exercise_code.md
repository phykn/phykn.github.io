---
title:  "딥러닝기초#4 경사하강법 연습 (코드)"
date: 2020-10-10 01:00
categories: [Education]
search: true
---
## 개요
앞에서 배운 경사하강법을 파이썬 코드로 구현해 보겠습니다. 아래는 모델 정의 및 학습과 관련된 부분을 작성한 코드 입니다. 코드를 통해 경사하강법이 어떻게 적용되는지 자세히 알아보겠습니다. <a href="https://github.com/phykn/example_code/blob/main/dl_basic/mnist_gd.ipynb">[전체 코드 바로가기]</a>

## 모델 초기화
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
NET 이라는 이름의 클래스를 만들고 모델을 초기화 합니다. 784개 입력과 10개 출력을 연결할 가중치 $w_{ij}$ 를 `self.w` 로 만들고 편향 $b_{j}$ 는 `self.b` 로 만들었습니다. `lr` 은 학습률입니다.

## 확률 계산
```python
    def forward(self, x):
        self.z = np.dot(x, self.w) + self.b
        self.p = self.sigmoid(self.z)
        return self.p
```
`forward` 함수는 확률을 계산합니다. 입력 (`x`) 를 받아 가중치 (`self.w`) 와 편향 (`self.b`) 으로 $z$ (`self.z`) 를 계산하고 여기에 활성함수인 시그모이드 (`self.sigmoid`)를 적용해 확률 (`self.p`) 을 얻습니다. 시그모이드는 아래와 같이 간단히 구현할 수 있습니다.

```python
    def sigmoid(self, x):
        return 1/(1+np.exp(-x))
```

## 변수 변경
앞에서 얻은 경사하강법 규칙은 아래와 같습니다.

$$
w_{ij} \gets w_{ij} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j}) x_{i}, \quad b_{j} \gets b_{j} - \alpha (p_{j} - t_{j}) p_{j} (1- p_{j})
$$

아래 `update` 함수는 경사하강법을 사용해 변수를 바꿉니다. 
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

## 결과
아래는 총 60,000 개 훈련 데이터로 학습시키며 오차를 계산한 그림입니다. 데이터가 입력될 수록 모델이 학습되어 오자가 작아져 0 부근의 점이 점점 많아지는 것을 확인 할 수 있습니다. 10,000 개의 테스트 데이터로 검증한 이 모델의 정확도는 88.57 % 입니다.
<center><img src="/assets/images/education/graph.png"></center>
<center style="font-size:15px">학습 오차</center><br>

딥러닝 라이브러리를 사용하면 보다 짧은 코드로 구현할 수 있습니다. 다음 페이지에서는 Tensorflow를 사용해 같은 모델을 만들고 학습해 보겠습니다.