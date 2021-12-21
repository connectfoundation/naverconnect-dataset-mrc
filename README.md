#### This dataset is created by NAVER Connect Foundation. 
# MRC(Machine Reading Comprehension)

## Overview

MRC 데이터셋은 질문에 대한 답변을 지문속에서 찾거나, 답변이 가능한지 판단하는 태스크(Machine Reading Comprehension)를 위한 데이터입니다.

(*해당 데이터셋은 한국어 자연어 이해 평가 벤치마크인 [KLUE](https://klue-benchmark.com/) 데이터셋의 일부이나, 기존 벤치마크와는 다름을 알립니다.*)

<br>

## 목차
1. 특징
2. 포맷
3. 구성
4. 라이센스 

<br>

## 1. 특징

MRC 데이터셋은 질의 응답에 관한 영문 벤치마크인 [SQuAD v1](https://rajpurkar.github.io/SQuAD-explorer/explore/1.1/dev/) 및 [SQuAD v2](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/)로부터 영감을 받아 제작되었습니다.

제공되는 문제 유형은 **패러프레이즈**를 거쳐 본문과 표현이 다른 질문과, 지문에서 **두 개 이상의 개념을 논리적으로 결합**해야 답을 추론할 수 있는 질문으로, 답변으로 가능한 후보들이 리스트 형태로 주어집니다. 즉, 학습된 모델이 추론하는 텍스트가 답변 후보에 있으면, 정답으로 인정받게 됩니다. 


<br>

## 2. 포맷

MRC 데이터셋은 Hugging Face에서 제공하는 [datasets](https://huggingface.co/docs/datasets/) 라이브러리를 이용하여 접근이 가능합니다. 저장된 디렉토리에서 아래의 코드를 활용하여 데이터셋을 불러올 수 있습니다. 
```
from datasets import load_from_disk
train_dataset = load_from_disk("./data/train_dataset/")

>>> print(train_dataset)
DatasetDict({
    train: Dataset({
        features: ['title', 'context', 'question', 'id', 'answers', 'document_id', '__index_level_0__'],
        num_rows: 3952
    })
    validation: Dataset({
        features: ['title', 'context', 'question', 'id', 'answers', 'document_id', '__index_level_0__'],
        num_rows: 240
    })
})
```
데이터셋은 편의성을 위해 Hugging Face에서 제공하는 datasets를 활용하여 pyarrow 형식으로 저장되어 있습니다. 데이터는 파싱을 통해 다음과 같은 형태로 변환됩니다.

```
{
'title': '인사조직관리',
'context': "'근대적 경영학' 또는 '고전적 경영학'에서 현대적 경영학으로 전환되는 시기는 1950년 대이다. 2차 세계대전을 마치고, 6.25전쟁의 시기로 유럽은 전후 재건에 집중하고, 유럽 제국주의의 식민지가 독립하여 아프리카, 아시아, 아메리카 대륙에서 신생국가가 형성되는 시기였고, 미국은 전쟁 이후 경제적 변화에 기업이 적응을 해야 하던 시기였 다. 특히 1954년 피터 드러커의 저서 《경영의 실제》는 현대적 경영의 기준을 제시하여서, 기존 근대적 인사조직관리를 넘어선 현대적 인사조직관리의 전환점이 된다. 드러커는 경영자의 역할을 강조하며 경영이 현시대 최고의 예술이자 과학이라고 주장하였고 , 이 주장은 21세기 인사조직관리의 역할을 자리매김했다.\\n\\n현대적 인사조직관리와 근대 인사조직관리의 가장 큰 차이는 통합이다. 19세기의 영향을 받던 근대적 경영학(고전적 경영)의 흐름은 기능을 강조하였지만, 1950년대 이후의 현대 경영학은 통합을 강조하였다. 기능이 분화된 '기계적인 기업조직' 이해에서 다양한 기능을 인사조직관리의 목적, 경영의 목적을 위해서 다양한 분야를 통합하여 '유기적 기업 조직' 이해로 전환되었다. 이 통합적 접근방식은 과정, 시스템, 상황을 중심으로 하는 인사조 직관리 방식을 형성했다.",
'question': '현대적 인사조직관리의 시발점이 된 책은?',
'id': 'mrc-0-004397', 
'answers': {'answer_start': [212], 'text': ['《경영의 실제》']},
'document_id': 51638 
 }
  ```
  
 각 성분에 대한 설명 및 type, 그리고 예시 데이터는 다음과 같으며, **answers** 성분은 두 가지의 하위 항목을 가지고 있습니다.

| 설명   대상          | 데이터 타입 |    설명                                                               |    예시 데이터                                             |    비고                                                  |
|:----------------------:|:-------------:|---------------------------------------------------------------------------|:----------------------------------------------------------------:|--------------------------------------------------------------|
| **title**                | str         | 지문이 발췌된 문서의 제목입니다.                                          | *'인사조직관리'*                         |                                                              |
| **context**              | str         | 문제가 출제되는 지문입니다.                                               | *'근대적 경영학' 또는 ... 직관리 방식을 형성했다.'*                        |                                                              |
| **question**             | str         | 지문을 보고 풀어야 할 질문(문제)입니다.                                   | *'현대적 인사조직관리의 시발점이 된 책은?'* |                                                              |
| **id**                 | str         | 해당 인스턴스의 고유 아이디입니다.                                        | *'mrc-0-004397'*                                     |                                                              |
|**answers**|| *{'text', 'answer_start'}* |
| *text*         | str         | answers의 하위 항목으로, 답변에 해당하는 텍스트입니다.                     | [*'《경영의 실제》'*]                                         | 답변은 여러 개 존재 가능                                     |
| *answer_start* | int       | answers의 하위 항목으로, 답변이 시작하는 지점의 위치를 나타냅니다.         | [212]                                                     | 답변이 여러 개 존재할 때, 각각의 answer_start                |
| **document_id** | int       | 해당 문서의 번호를 나타냅니다.         | 51638                                                     |                 |

 
  

## 3. 구성
다음은 데이터의 구성입니다.
```
# 전체 데이터
./data/
  # 학습에 사용할 데이터셋. train과 validation으로 구성
  ./train_dataset/
    # 학습용 train set
    ./train/
    # 학습용 validation set
    ./validation/
    # train 과 validation으로 구성되어 있음
    ./dataset_dict.json
  # 위키피디아 문서 말뭉치. retrieval을 위해 사용됨
  ./wikipedia_documents.json
```

학습 데이터는 train data, validation data, 그리고 두 가지 데이터가 제공됨을 알려주는 *dataset_dict.json*으로 구성되며, 명세는 다음과 같습니다.
 
 | 세부 분류 | 샘플 수 | 용도 | 공개 여부 |
 |:----------:|:---------:|:-----:|:-----------:|
 | **train**    | 3,952   |학습용|모든 정보 공개 (id, question, context, answers, document_id, title)|
 |**validation**|240      | 학습용 |   (train과 동일)     |
  
 또한, 추가적으로 retrieval 과정에서 사용하는 말뭉치는 *./data/wikipedia_documents.json* 으로 저장되어 있습니다. 약 5만 7천개의 겹치지 않는 문서들로 구성되어 있습니다.
 
 ```
 import pandas as pd
 wiki =  pd.read_json('data/wikipedia_documents.json')
 
 >>> wiki[0]
text             이 문서는 나라 목록이며, 전 세계 206개 나라의 각 현황과 주권 승인 정보를 개...
corpus_source                                                위키피디아
url                                                           TODO
domain                                                        None
title                                                        나라 목록
author                                                        None
html                                                          None
document_id                                                      0
 ```
  
  
<br>

 ## 4. 라이센스
 
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
  
  ### Contact
 
 작성자: 조원익 tsatsuki@snu.ac.kr
