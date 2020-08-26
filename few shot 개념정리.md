# Few Shot Learning 개념정리

meta learning은 몇몇 training 예제를 통해 모델로 하여금 새로운 기술을 배우거나, 새로운 환경에 빠르게 적응할 수 있도록 설계하는 것은 나타냄. 이런 meta learning을 학습하는 방법 중 하나가 few-shot learning.

Few-shot learning의 경우 classfication과는 다른 방법으로 문제 및 데이터셋 정의.

### Few-Shot Learning

정의 : Few-Shot Learning is an example of meta-learning, where a learner is trained on several related tasks, during the meta-training phase, during the meta-training phase, so that it can generalize well to unseen (but related) tasks with just few examples, during the meta-testing phase. An effective approach to the Few-Shot Learning problem is to learn a common representation for various tasks and train task specific classifiers on top of this representation.

간단 정의 : 아주 적은 데이터로도 데이터의 특징을 식별하도록 하는것.

Few-Shot Learning의 수행 프로세스 : 기존 deep learning의 경우, 수백장 사진을 통해 강아지라는 class의 특성을 학습 → 사람이 한 장의 강아지의 사진을 통해 강아지라는 class의 concept를 학습할 수 있는 것처럼 network 학습시에도 class 별로 적은 양의 이미지를 보여주어 network를 학습시키고 학습한 network를 테스트

데이터 수가 매우 적은 Few Shot Learning task에서는 데이터셋을 train에 사용하는 서포트 데이터(support data)와  test에 사용하는 쿼리 데이터(query data)로 구성  → 이런 Few-Shot Leaning task를 'N-way K-shot 문제'라고 함



### Few-Shot Learning 관련 주요 용어

- n-way : 각 batch 별로 선택하는 label의 갯수(N)
- K-shot : 각 class 별로 선택하는 Data의 갯수(K)
- support data : 해당 batch에서 fine tuning을 위해 학습하는 데이터
- query data : 해당 batch에서 class를 예측(predict/test)해야 하는 데이터
- episode : 한 epoch 당 수행하는 iteration 횟수



### Few-shot Data 생성 프로세스

1. 가장 처음에는 Train/Test/(Valid) Set으로 데이터를 분활. (기존 알고리즘 학습을 위한 방법과 동일)
   ![전체적인 데이터 구조](https://seujung.github.io/files/180622_meta_learning/fig1.png)

2. 그 다음에는 각각 Data에서 N개의 class에 대해 sampling 작업 수행
   ![Train Data 구조 1](https://seujung.github.io/files/180622_meta_learning/fig2.png)

3. sampling한 label을 기준으로 하여 k개씩 데이터를 생성. 이때 2가지 dataset(support/query)을 생성. 해당 데이터를 기준으로 support data로 모델 학습을 먼저 수해하고, 모델에 대한 평가는 query로 판단
   ![Train Data 구조 2](https://seujung.github.io/files/180622_meta_learning/fig3.png)

**주의할점**

Few-shot Learning에서 기존 데이터 생성과의 가장 큰 차이점은 Sampling 수행 시 label의 index가 계속 바뀜. 

예시 : CIFAR-10의 경우

- 기존 모델 학습의 경우 : airplane = 0 ,…,truck = 9로 label에 대한 index를 부여
  -  이 label은 동일한 상태로 유지
-  Few-Shot Learning의 경우 : sampling 에 따라 airplane의 label이 0이 될 수도 있고 2가 될 수 있음.
  - classification가 가장 차별된 점
  - But,  sampling 돤 데이터에서 support와 query data는 동일한 label을 유지 → 모델이 어떤 데이터가 어떤 label을 의미하는 지 알고 예측할 수 있기 때문



### few shot learning 연구 흐름

1. 풀려고 하는 task와 비슷한 task의 학습 모델이 있는 경우 → 초기 파라미터는 이미 만들어진 학습모델의 것을 가져와서 쓰되 데이터를 가지고 미세한 튜닝을 함
2. 고차원 학습데이터를 저차원 공간으로 옮겨와 고차원 공간에서의 데이터와 저차원 공간에서 데이터 사이의 유사도를 기준으로 분류하는 방식
3. GAN 등 생성 모델을 사용해 데이터를 늘리는 방식



### 참고 문서

[meta learning - Few Shot Learning](https://seujung.github.io/2018/06/29/meta_learning_1/)

[few shot learning) few shot learning이란? #1](https://curiousseed.tistory.com/46)

[Meta-Learning: Learning to Learn Fast](https://talkingaboutme.tistory.com/entry/DL-Meta-Learning-Learning-to-Learn-Fast)

https://www.kakaobrain.com/blog/106

https://paperswithcode.com/task/few-shot-learning



