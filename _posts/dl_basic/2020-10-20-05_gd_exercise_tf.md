---
title:  "딥러닝기초#5 경사하강법 연습 (Tensorflow)"
date: 2020-10-20 00:00
categories: [Education]
search: true
---
## 1. 개요
텐서플로우 (Tensorflow) 는 딥러닝 라이브러리로 자동으로 미분 계산을 처리합니다. 따라서 짧은 코드로도 딥러닝 학습이 가능합니다. 이번 시간에는 [딥러닝기초#4 경사하강법 연습 (코드)](..\04_gd_exercise_code) 와 같은 모델을 텐서플로우로 구현해보면서 어떤 차이점이 있는지 살펴보겠습니다. <a href="https://github.com/phykn/example_code/blob/main/dl_basic/mnist_gd_tf.ipynb">[전체 코드 바로가기]</a>

## 2. 모델 만들기
```python
def create_dnn_model():
    inputs = tf.keras.layers.Input(shape=(784))
    x = tf.keras.layers.Dense(10, activation='sigmoid')(inputs)
    model = tf.keras.models.Model(inputs=inputs, outputs=x)    
    return model
```
`create_dnn_model` 은 모델을 만드는 함수입니다. 모델은 `Input` 을 통해 길이 784 의 데이터를 입력받습니다. `Dense` 레이어는 인공신경망을 의미합니다. `Dense` 의 인자로 출력 길이 (10) 와 활성함수 `sigmoid` 를 지정했습니다. 

```python
model.compile(loss='mse', optimizer='sgd')
```
모델을 훈련하기 전에 `compile` 명령어로 모델을 컴파일 합니다. 손실함수로 `mse` 를, 옵티마이저는 경사하강법에 해당하는 `sgd` 를 입력합니다. 텐서플로우는 모델 구조와 손실함수를 지정하면 자동으로 미분을 계산해 주기 때문에 경사하강법 규칙은 작성하지 않아도 학습이 가능합니다.

## 3. 모델 훈련
텐서플로우에서는 `fit` 명령어로 간단히 훈련을 수행할 수 있습니다. 이 예시에서는 [딥러닝기초#4 경사하강법 연습 (코드)](..\04_gd_exercise_code) 와 같이 개별 데이터에 대한 에러를 살펴보기 위해 `train_on_batch` 명령어를 사용합니다. (데이터가 하나씩 입력되어 느리게 작동합니다.)

## 4. 결과
[딥러닝기초#4 경사하강법 연습 (코드)](..\04_gd_exercise_code) 와 마찬가지로 데이터가 입력될 수록 오차가 작아져 0 부근으로 몰리는 것을 확인 할 수 있습니다. 10,000 개의 테스트 데이터로 검증한 모델의 정확도는 86.86 % 입니다.

<center><img src="/assets/images/education/graph_tf.png"></center>
<center style="font-size:15px">학습 오차</center><br>

