# 4장. 단어 수준 임베딩 : NPLM
[TOC]
## 1 NPLM
### 1.1 모델 기본 구조

- 모델구성도	![모델 구성도](C:\Users\huare\나의 공간\markdown\한국어 임베딩\4장\\1_NPLM\1.png)
- 통계기반의 전통적인 언어 모델의 한계 극복
  - 기존 언어모델의 단점
    1. 학습 데이터에 존재하지 않는 n-gram이 포함된 문장이 나타날 확률 값을 0으로 부여한다. 백오프나 스무딩으로 이런 문제를 일부 보완할 수 있지만 완전한 것은 아니다.
    2. 문장의 장기 의존성(long-term dependency)을 포착해내기 어렵다. 다시 말해 n-gram 모델의 n을 5이상으로 길게 설정할 수가 없다. n이 커질수록 그 등장 확률이 0인 단어 시퀀스가 기하급수적으로 늘어난다.
    3. 단어/문장 간 유사도 계산을 할 수 없다.
    
       NPLM은 이러한 기존언어 모델의 한계를 일부 극복한 언어모델이라는 점에서 의의가 있다. 그뿐만 아니라 NPLM 자체가 단어 임베딩 역활을 수행할 수 있다.
  
  - 언어 모델의 한계 극복이 가능했던 이유 → **분산표상**
    $$
    W\in { R }^{ \left| V \right|  }\quad \rightarrow \quad W\in { R }^{ n }\\ n<<\left| V \right|
    $$
    분산표상이 추구하는 바는 위 식과 같다. $W$는 단어 벡터를 말하고, one-hot-vector의 차원수는 사전의 전체 단어 수 ($V$)이다. 즉, $V$보다 훨씬 작은 $n$차원 벡터로 바꿔보자는 것. one-hot-vector는 그 요소가 0 혹은 1인 binary구조이지만, 분산표상으로 표현된 단어벡터의 요소는 연속형의 실수값이다. 즉, 듬성듬성한(sparse) 벡털르 빽빽한(dense) 벡터로 바꿔 표현한 것. 
  
    ![](C:\Users\huare\나의 공간\markdown\한국어 임베딩\4장\1_NPLM\2.png)
    왼쪽 그림을 보면 총 9개 개체가 존재. 이를 one-hot-vector로 표현한다면 9차원짜리 벡터 9개가 나오겠지만 **2개의 속성인 color와 shape로 설명, 특성을 나타낸다면 2차원 벡터로도 충분히 표현이 가능하다.** 이것이 바로 **분산표상**의 개념이다.
    
    분산 표상은 데이터의 차원수를 줄이려는 것이 1차적 목적이지만, 그효과는 엄청나다. 위 예시처럼 color나 shape를 기준으로 개체간 유사성을 잴 수 있듯이, 단어 벡터도 그 요소값이 연속적이기때문에 단어 벡터간에 거리나 유사도를 재는 것이 가능하다. **의미가 유사한 단어는 벡터공간에서 가깝게, 반대의 경우는 멀게 배치하는 것**이 **분산표상의 목표**이다.
    
    ![3](C:\Users\huare\나의 공간\markdown\한국어 임베딩\4장\1_NPLM\3.png)

### 1.2 학습

- 단어 시퀀스가 주어졌을 때 다음단어가 무엇인지 맞추는 과정에서 학습. 즉, 직전까지 등장하는 n-1개 단어들로 다음 단어를 맞추는 n-gram 언어 모델.

- 수식 4.1을 최대화

  $$
  P({ w }_{ t }|{ w }_{ t-1 },...,{ w }_{ t-n+1 })=\frac { exp({ y }_{ { w }_{ t } }) }{ \sum _{ i }^{  }{ exp({ y }_{ i }) }  } \\ \\ 수식 4.1
  $$

  - $w_t$ : 문장 t번째 단어
  - $exp(x)$ : $e^x$

- NPLM 구조 말단의 출력은 |V|차원의 스토어 벡터 $y_{wt}$에 softmax함수를 적용한 |V|차원의 확률벡터. 

- 입력 

  - $X = [x_{t-1},x_{t-2}, ..., x_{t-n+1}]$
    - $x_t$ : 문장 내 t 번째 단어 ( $w_t$ )에 대응하는 단어 벡터.
    - 즉, $x_t = C(w_t)$   → $|V|\times m$ 크기를 갖는 커다란 행렬 $C$에서 $w_t$에 해당하는 벡터를 참조^lookup^한 형태. $C$행렬의 원소값은 초기에 랜덤설정. $|V|$는 어휘집합크기, $m$은 $x_t$의 차원 수

- 학습

  - 입력층에서 은식층, 출력층을 거쳐 스코어 벡터 $y_{w_t}$를 계산한다.
  $$
  { y }_{ { w }_{ t } }=b+U\cdot tanh(d+H{ x }_{ t }) \\ 수식4.2	
  $$
  
  - $y_{w_t}$에 softmax를 적용한 뒤 정답단어의 인덱스와 비교해 역전파하는 방식으로 학습이 이루어진다. 
  
  - NPLM의 학습 파라미터는 아래와 같다.
    $$
    H\in { R }^{ h\times (n-1)m },\quad { x }_{ t }\in { R }^{ (n-1)\times m },\quad d\in { R }^{ h\times 1 }\\ U\in { R }^{ |V|\times h },\quad b\in { R }^{ |V| },\quad y\in { R }^{ |V| }\quad,C\in{R}^{m\times|V|}
    $$

### 1.3 NPLM과 의미 정보

`The cat is walking in the bedroom`

`A dog was running in a room`

`The cat is running in a room`

`A dog is walking in a bedroom`

`The dog was walking in the room`

위 5가지 문장을 이용해 NPLM이 어떻게 단어의 의미를 임베딩에 녹여낼 수 있는지 알 수 있다.

n-gram의 n을 4로 두면, NPLM은 직전 3개 단어를 가지고 그 다음 단어를 맞추는 과정에서 학습하게 된다. target 단어가 `walking`을 공유하는 3-gram은 아래 시퀀스와 같다.

`The cat is`

`A dog is`

`The dog was`

NPLM은 위 시퀀스를 각각 입력받으면 `walking`이 출력되도록 학습한다. 따라서 `The`, `A` , `cat`, `dog`, `is`, `was` 등은 `walking`이라는 단어와 모종의 관계가 형셩된다. 바꿔 말하면 `The`, `A` , `cat`, `dog`, `is`, `was` 등에 해당하는 $C$행렬의 행 벡터들은 `walking`을 맞추는 과정에서 발생한 학습 손실 ^train_loss^를 최소화하는 그래디언트^gradient^를 받아 동일하게 업데이트된다. 결과적으로   `The`, `A` , `cat`, `dog`, `is`, `was` 벡터가 벡터 공간에서 같은 방향으로 조금 움직인다는 이야기이다.



NPLM은 그 자체로 언어 모델 역활을 수행할 수 있다 . 예컨대 학습 데이터에 없는 `The mouse is running in a room`이라는 문장의 등장 확률을 예측해야 한다고 가정해보자. 기존의 통계 기반 n-gram 모델은 학습 데이터에 한번도 등장하지 않은 패턴에 대해서는 그 등장 확률을 0으로 부여하는 문제점을 가지고 있다.하지만 NPLM은 `The mouse is running in a room`이라는 문장이 말뭉치에 없어도 문맥이 비슷한 다른 문장을 참고해 확률을 부여한다. 결과적으로 NPLM은 `The mouse is running in a room`의 등장 확률을 `A dog was running in a room`, `A dog is walking in a bedroom`와 같은 문장들과 비슷하게 추론하게 되는 것이다. 