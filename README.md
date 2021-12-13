#### This dataset is created by NAVER Connect Foundation. 
# MRC(Machine Reading comprehension)

## Overview

MRC 데이터셋은 질문에 대한 답변을 지문속에서 찾거나, 답변이 가능한지 판단하는 태스크(Machine Reading Comprehension)를 위한 데이터입니다.

(*해당 데이터셋은 한국어 자연어 이해 평가 벤치마크인 [KLUE](https://klue-benchmark.com/) 데이터셋의 일부이며, 기존 벤치마크와는 다름을 알립니다.*)

<br>

## 목차
1. 특징
2. 포맷
3. 각 성분에 대한 소개
4. 라이센스 

<br>

## 1. 특징

MRC 데이터셋은 질의 응답에 관한 영문 벤치마크인 [SQuAD v1](https://rajpurkar.github.io/SQuAD-explorer/explore/1.1/dev/) 및 [SQuAD v2](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/)로부터 영감을 받아 제작되었습니다.

제공되는 문제 유형은 세 가지로, 1) **패러프레이즈**를 거쳐 본문과 표현이 다른 질문, 2) 지문에서 **두 개 이상의 개념을 논리적으로 결합**해야 답을 추론할 수 있는 질문, 그리고 3) **지문만으로는 답변을 알 수 없는 질문**이 있습니다. 

1-2)에 대해서는 답변으로 가능한 후보들이 리스트 형태로 주어집니다. 즉, 학습된 모델이 추론하는 텍스트가 답변 후보에 있으면, 정답으로 인정받게 됩니다. 

3)에 대해서는, 문제 제작자들이 '함정으로 생각한 답변'이 제공되며, 실제로는 지문 내의 어떤 텍스트도 정답으로 추론하지 않아야 합니다(정답 없음). 3)에 해당하는 문항의 예시는 다음과 같습니다.

지문:
```
개인정보 유출 피해자들이 업체를 상대로 낸 집단소송에서 법원이 손해배상 판결을 내린 것은 이번이 처음이다. 피해자 3500만명이 이 판결과 같은 결과를 얻게 된다면 위자료는 7조원에 이를 수도 있다. 그러나 한국은 소송을 내지 않은 피해자도 배상을 받는 ‘집단소송제’를 도입하지 않았다. 위자료를 받으려면 본인이 직접 소송을 해야 한다. 2011년 7월 네이트·싸이월드 회원 정보가 유출된 이후 피해자들이 SK커뮤니케이션즈를 상대로 낸 손해배상 청구소송은 20여건에 달하지만 한 건을 제외하고는 모두 패소했다. 지난해 4월 대구지법 구미시법원(판사 임희동)은 유능종 변호사(48)가 SK컴즈를 상대로 낸 소송에서 “위자료 100만원을 지급하라”고 원소 승소 판결했다. 그러나 서울중앙지법은 지난해 11월 이모씨 등 2847명이 SK컴즈 이스트소프트 등을 상대로 낸 소송에서 원고 패소 판결을 내렸다. 앞으로 진행될 소송에선 해킹을 막지 못한 SK컴즈의 과실 여부를 둘러싸고 치열한 법적 공방이 예상된다. 15일 재판에선 △10기가바이트(GB) 크기의 개인 정보가 해킹으로 유출됐는데 이를 SK컴즈가 탐지하지 못한 점 △기업용 알집프로그램보다 해킹에 취약한 공개용 알집프로그램을 사용한 점 △개인 정보 관리자가 퇴근 후 로그아웃을 하지 않아 해커가 서버에 쉽게 접근할 수 있었던 점 등을 SK컴즈 측의 과실로 인정했다. 다른 기업들의 개인정보 유출 사고도 관심을 끈다. 2008년 1월 옥션에서 1860만명, 이듬해 9월 GS칼텍스에서 1125만명의 고객정보가 유출된 것을 시작으로 2011년 4월 현대캐피탈(175만명), 11월 넥슨(1320만명), 2012년 7월 KT(873만명) 등에서 개인정보가 유출되는 사고가 터졌다. 옥션 GS칼텍스 KT 등을 상대로 일부 피해자들이 제기한 재판이 끝났거나 현재 진행 중이다. 넥슨과 현대캐피탈 등도 손해배상 소송에 휘말릴 가능성이 있다.	
```

질문:
```
배상 판결이 나 집단 전체가 배상을 받을 수 있게 한 국내 제도는?
```

가짜 답변(answer):
```
집단소송제
```

하지만, 지문에 있는 다음 구절을 통해 질문이 해당 지문만으로는 답변할 수 없음을 알 수 있습니다.

```
그러나 한국은 소송을 내지 않은 피해자도 배상을 받는 ‘집단소송제’를 도입하지 않았다.
```

이처럼, 3)의 문항은 모델이 지문의 내용을 모두 파악하여 해당 질문을 답변 가능한지 판단해야 합니다. 이는 [SQuAD v2](https://rajpurkar.github.io/SQuAD-explorer/explore/v2.0/dev/)를 벤치마킹한 것이며, MRC 태스크의 난이도를 한층 높여 줍니다.

상세한 문제 유형 분포 및 KorQuAD 데이터셋과의 비교 실험은 [KLUE 논문](https://arxiv.org/abs/2105.09680)의 machine reading comprehension 섹션에서 확인하실 수 있습니다.

<br>

## 2. 포맷

MRC 데이터셋은 *.json* 형식으로 제공되며, 다음과 같은 구조를 가지고 있습니다.

```
{'answers': {'answer_start': [478, 478], 'text': ['한 달가량', '한 달']},
 'context': '올여름 장마가 17일 제주도에서 시작됐다. 서울 등 중부지방은 예년보다 사나흘 정도 늦은 이달 말께 장마가 시작될 전망이다.17일 기상청에 따르면 제주도 남쪽 먼바다에 있는 장마전선의 영향으로 이날 제주도 산간 및 내륙지역에 호우주의보가 내려지면서 곳곳에 100㎜에 육박하는 많은 비가 내렸다. 제주의 장마는 평년보다 2~3일, 지난해보다는 하루 일찍 시작됐다. 장마는 고온다습한 북태평양 기단과 한랭 습윤한 오호츠크해 기단이 만나 형성되는 장마전선에서 내리는 비를 뜻한다.장마전선은 18일 제주도 먼 남쪽 해상으로 내려갔다가 20일께 다시 북상해 전남 남해안까지 영향을 줄 것으로 보인다. 이에 따라 20~21일 남부지방에도 예년보다 사흘 정도 장마가 일찍 찾아올 전망이다. 그러나 장마전선을 밀어올리는 북태평양 고기압 세력이 약해 서울 등 중부지방은 평년보다 사나흘가량 늦은 이달 말부터 장마가 시작될 것이라는 게 기상청의 설명이다. 장마전선은 이후 한 달가량 한반도 중남부를 오르내리며 곳곳에 비를 뿌릴 전망이다. 최근 30년간 평균치에 따르면 중부지방의 장마 시작일은 6월24~25일이었으며 장마기간은 32일, 강수일수는 17.2일이었다.기상청은 올해 장마기간의 평균 강수량이 350~400㎜로 평년과 비슷하거나 적을 것으로 내다봤다. 브라질 월드컵 한국과 러시아의 경기가 열리는 18일 오전 서울은 대체로 구름이 많이 끼지만 비는 오지 않을 것으로 예상돼 거리 응원에는 지장이 없을 전망이다.',
 'guid': 'klue-mrc-v1_train_12759',
 'is_impossible': False,
 'news_category': '종합',
 'question': '북태평양 기단과 오호츠크해 기단이 만나 국내에 머무르는 기간은?',
 'question_type': 1,
 'source': 'hankyung',
 'title': '제주도 장마 시작 … 중부는 이달 말부터'}
  ```
  
## 3. 각 성분에 대한 소개
각 성분에 대한 설명 및 type, 그리고 예시 데이터는 다음과 같습니다. **굵은 글씨**로 표시된 성분들은 태스크 해결에 필수적인 요소들이며, **answer** 성분은 두 가지의 하위 항목을 가지고 있습니다. 알파벳 순서로 제공되는 *.json* 형식과 달리, 여기서는 실제 문제 해결에 사용되는 양상에 따라 성분들을 배열하였습니다. 

| 설명   대상          | 데이터 타입 |    설명                                                               |    예시 데이터                                             |    비고                                                  |
|----------------------|-------------|---------------------------------------------------------------------------|----------------------------------------------------------------|--------------------------------------------------------------|
| **guid**                 | str         | 해당 인스턴스의 고유 아이디입니다.                                        | *'klue-mrc-v1_train_12759'*                                     |                                                              |
| source               | str         | 지문이 발췌된 텍스트의 유형입니다.                                        | *'hankyung'*                                                      | 뉴스의 경우 한국경제 혹은 아크로팬이며, 그 외에는 위키피디아 |
| news_category        | str         | 뉴스의 경우, 카테고리 정보가 포함되어 있습니다.                           | *'종합'*                                                           | source가 언론사일 경우, 해당 언론사에서 제공된 카테고리 정보 |
| title                | str         | 지문이 발췌된 문서의 제목입니다.                                          | *'제주도 장마 시작 … 중부는 이달 말부터 '*                         |                                                              |
| **context**              | str         | 문제가 출제되는 지문입니다.                                               | *'올여름 장마가 ... 지장이 없을 전망이다.'*                        |                                                              |
| **question**             | str         | 지문을 보고 풀어야 할 질문(문제)입니다.                                   | *'북태평양 기단과 오호츠크해 기단이 만나 국내에 머무르는 기간은?'* |                                                              |
| question_type        | int32       | 질문의 유형을 나타냅니다.                                                 | 1                                                              | 총 세 개의 질문 유형(1-3)이 존재하며, 3은 unanswerable          |
| **is_impossible**        | boolean     | 질문이 해당 지문을 읽고 답할 수 있는가(unanswerable)의 여부를 나타냅니다. | FALSE                                                          | TRUE일 경우 unanswerable                                     |
|**answer**|| *{'text', 'answer_start'}* |
| **text**         | str         | answer의 하위 항목으로, 답변에 해당하는 텍스트입니다.                     | [*'한 달가량'*, *'한 달'*]                                         | 답변은 여러 개 존재 가능                                     |
| **answer_start** | int32       | answer의 하위 항목으로, 답변이 시작하는 지점의 위치를 나타냅니다.         | [478, 478]                                                     | 답변이 여러 개 존재할 때, 각각의 answer_start                |


- 데이터셋 사이즈는 17,554개입니다.
- 원시 코퍼스: 위키피디아 / 뉴스 (한국경제, Acrofan)
- 크라우드소싱: [셀렉트스타](https://selectstar.ai/)
 
<br>

 ## 4. 라이센스
 
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
  
  ### Contact
 
 작성자: 조원익 tsatsuki@snu.ac.kr
