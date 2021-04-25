# Gradient Descent VS Stochastic Gradient Descent

[TOC]

## Gradient Descent

![img](https://t1.daumcdn.net/cfile/tistory/99E6363359D86A8805)

전체 데이터에 대한 loss값을 구한 뒤, 그 loss의 미분(Gradient Descent)을 통해 weight를 업데이트 하여, 에러를 계속 줄이면도록 학습.

![img](https://t1.daumcdn.net/cfile/tistory/99EC803359D86AF115)

> Weight 업데이트 = 에러 낮추는 방향(decent)(-) X 한발자국 크기(learning reate)(r) X 현 시점의 기울기 (gradient)(기울기 값)

하지만 weight 업데이트를 한번 할때마다 모든 데이터셋 계산을 해야하기때문에 학습 속도가 느리다는 단점을 가짐 → SGD가 이를 보완



## Stochastic Gradient Descent

Gradient Descent처럼 모든 데이터에 대한 loss값을 계산한 뒤 weight를 업데이트 하는 것이 아니라, 적은 데이터에 대한 loss값을 계산한뒤 weight 업데이트.

![img](https://t1.daumcdn.net/cfile/tistory/999EA83359D86B6B0B)



## GD VS SGD

### GD

- 모든 데이터를 계산한뒤 weight 업데이트
- 최적의 한 스텝을 나아감
- 확실한데 너무 느림

### SGD

- 일부 데이터를 계산한위 weight 업데이트
- 빠르게 전진
- 헤맴. 방향이 뒤죽박죽
  ![img](https://blog.kakaocdn.net/dn/b8RUdK/btqAm4Utey2/5DfjTQw70XKYgtOB8VHpeK/img.png)



## Optimizer 계보

![img](https://t1.daumcdn.net/cfile/tistory/993D383359D86C280D)

> 위의 자료는 references에 나와있는 하용호님의 슬라이드에 있는 내용입니다. Optimizer에 대해서 굉장히 손쉽고 이해하기 쉽게 정리를 해주셨습니다.



![](http://i.imgur.com/pD0hWu5.gif?1)



## 결론

- GD
  - 최적값을 찾아가는 것은 정확하지만, 속도가 느림
- SGD
  - 방향 뒤죽박죽. 또한 적절한 learning rate를 결정하는 것이 중요.
- 방향과 스텝 사이즈(learning rate)를 고려한 새로운 Optimizer
  - 방향성
    - Momentum
    - NAG
  - 스텝 사이즈(learning rate)
    - Adagrad
    - RMSPop
    - AdaDelta
  - 방향성 + 스텝 사이즈(learning rate)
    - **Adam**
    - Nadam



## 참조사항

- Mini-Batch Gradient descent는 [이 자료](https://light-tree.tistory.com/133) 참조



## 참조 자료

- https://seamless.tistory.com/38
- https://www.slideshare.net/yongho/ss-79607172

- https://light-tree.tistory.com/133

- https://go-hard.tistory.com/11

