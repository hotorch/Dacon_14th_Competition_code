# Dacon 14회 금융문자 분석 경진대회

## 모델
- 자연어처리에서 임베딩을 하는 것은 매우 중요한 작업이며, 이에 대한 접근법은 크게 3가지가 있습니다. (출처 : 한국어 임베딩, 이기창 저)
   - a. Language Model : 단어가 어떤 순서로 쓰였는가? (ELMo, GPT, etc)
   - b. Distributional hypothesis : 어떤 단어가 동시에 출현하여 같이 쓰였는가? (word2vec, glove etc)
   - c. Bag-of-Words hypothesis : 어떤 단어가 많이 쓰였는가? (TF-IDF etc)
- 저희는 주어진 Task에 잘 맞는 **매우 심플한 모델**을 구성하였습니다.
- 모델은 **1. TF-IDF** / **2. LightGBM** 순서로 이루어져 있습니다.

### 1. TF-IDF
- 행에는 스미싱 문자들의 정보, 열에는 용어 정보가 나열되고 개별 문자에서 발생한 용어의 발생 빈도를 원소로 하는 행렬을 **DTM(Document-Term frequency Matrix)**이라고 부릅니다.
- 하지만 DTM의 용어들의 빈도만을 가지고 문자들의 차별성과 용어들의 중요도를 반영하기 어려훠 대중적으로 많이 사용하는 용어 가중치인 **TF-IDF**를 적용합니다.
- 용어의 빈도수의 영향이 크다고 판단하여 TF 항을 **log(1+TF)으로 변환**하여 사용하였습니다.(Weight Functions Impact on LSA Performance, Proceeding of the Recent Advances in Natural language processing, 2001)
- 가중치를 반영한 DTM(270K * 10K)를 바탕으로 **머신러닝 기법(LightGBM)**을 실시합니다.

### 2. LightGBM
- TF-IDF DTM을 바탕으로 자체적으로 머신러닝 기법 10가지 정도 간단히 테스트 했을 때 LightGBM이 가장 성능이 좋아 Baseline Model로 선택하였습니다.
- Parameter는 Grid Search를 실시하여 n_estimators, learning_rate, min_child_samples, max_depth 들의 큰 틀을 잡았습니다.
- 세부적인 튜닝의 우선 순위는 다음과 같습니다.
   - 우선순위 1. n_estimators
   - 우선순위 2. learning_rate
   - 우선순위 3. min_child_samples
   - 우선순위 4. max_depth

## Requirements
### 1. 개발 환경
- Window 10, Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz, RAM 16.GB, 64bit
- conda 4.8.1, Python 3.7.3, IPython 7.6.1, Jupyter Notebook 6.0.0

### 2. 필요 라이브러리
- numpy
- pandas
- konlpy
- lightgbm 
- sklearn 


## 파일 설명 및 구조
   - train.csv : 훈련 데이터입니다. 데이터는 공유가 불가능합니다.
   - public_test.csv : 가채점 테스트 데이터입니다. 데이터는 공유가 불가능합니다.
   
- 데이터 설명 : https://newfront.dacon.io/competitions/official/235401/data?join=1
- 다운로드 : 스폰서의 요청으로 대회 종료 후에는 데이터를 다운 받을 수 없습니다

~~~
├──  README.md
├──  code_notebook.ipynb
~~~
   
## Author
- Ho Young Jeong @hotorch
- Author Email : ghdud1519@naver.com


## Result
- Overall Rank : 1st Place (스팸구이)
- Reference : https://dacon.io/competitions/official/235401/leaderboard/
