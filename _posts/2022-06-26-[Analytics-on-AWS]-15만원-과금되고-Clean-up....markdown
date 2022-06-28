---
layout: post
title:  "[Analytics on AWS] 15만원 과금되고 Clean up..."
author: 김예나
date:   2022-06-26 22:00:00 +0900
categories: [aws]
tags: [analytics on aws]
math: true
mermaid: true
---


최근 Analytics on AWS workshop를 진행하며 처음에는 몇만 원 과금되었던 것이 눈덩이처럼 불어더니 결국 10만 원을 넘어갔고, 그제서야 무언가 잘못되었다는 것을 깨달았으나 대체 뭘 해야할지 몰라 일단 두었습니다.(당시 Warehouse on Redshift 과정 하나만 남겨두고 있었는데, 얼른 끝내고 Clean up 해버리자는 생각으로...) 하지만 선배님을 통해 이런 경우 1회에 한하여 실수로 과금된 요금을 환불받을 수도 있다는 사실을 알게 되었고 과금된 요금부터 확인했습니다.


![image](https://user-images.githubusercontent.com/80688900/175816431-e31c0e69-c1bb-43c5-8eca-621785bd1034.png)


청구서를 열어봤습니다.


![image](https://user-images.githubusercontent.com/80688900/175816491-f3c06842-0f73-4584-a1e7-140487c24b3e.png)


확인해보니 대부분이 AWS Glue StartDevEndpoint에서 청구된 것이었습니다. 나머지는 아마 workshop을 꽤 오래 끌면서 공부해서 과금된 것 같습니다. 우선 Glue 콘솔에 들어가 Endpoint라고 되어있는 곳을 확인했는데, 이미 아무것도 없는 상태였습니다. 뭐지? 싶어 무작정 고객센터에 문의를 넣었습니다.


![image](https://user-images.githubusercontent.com/80688900/175816649-ac536ea1-a680-44fe-b03e-54515557fd2b.png)


위쪽 바에 있는 물음표를 누르면 지원 센터에 들어갈 수 있습니다. 내용은 영어로 작성해야 했기에 영어를 잘하는 편은 아니지만 열심히 제 상황을 설명하고 환불받을 수 있는지를 물어봤습니다.


![image](https://user-images.githubusercontent.com/80688900/175816756-de1fe913-4c4c-48e8-8125-68514f32ee3a.png)


다음 날 아침에 답변이 도착했는데, 대충 요약하자면 아직 Glue 서비스가 종료되지 않았다. 1번의 예외로 환불은 가능하지만 종료를 먼저 한 후에 도와주겠다는 내용이었습니다. 그리고 다음과 같이 종료하는 법이 써있었습니다.


![image](https://user-images.githubusercontent.com/80688900/175816892-d894aa51-90af-4d35-bb21-1aeab28132d5.png)


하지만, 정작 콘솔에 들어가 보니 아무것도 없었습니다(...)


그래서 그냥 빠르게 Warehouse on Redshift까지 마저 하고 Clean up 해야겠다고 마음먹은 그 날, 절망적이게도 비가 쏟아지는 날 노트북을 들고 나갔다가 집에 들어왔더니 작동이 되지 않았습니다. 2 ~ 3일 기다리면서 노트북을 말리려고 생각하니 도저히 안 되겠어서 그냥 그 길로 동생 노트북을 빌려 Clean up을 쭉 진행했습니다.


![image](https://user-images.githubusercontent.com/80688900/175817248-1d87a287-d9eb-491d-b5d4-79e5e05168c6.png)


그러다 찾은 SageMaker 노트북. 세 번째 단계인 Transform Data with AWS Glue에서 잠깐 사용했던 것이 종료되지 않고 계속 켜져 있었습니다. Glue 콘솔에서 ETL - Notebooks(legacy)에 있습니다. 즉시 노트북을 중지하고 삭제했습니다.


여기까지 진행한 후 문의에 답장했더니 AWS의 공동 책임 모델 문서를 읽고 동의해 달라고 하셨고, 동의한다는 내용의 답을 보내고 나니 주말이 지나고 답장이 도착했습니다.(크레딧과 함께!)


![image](https://user-images.githubusercontent.com/80688900/176157831-5ef0a5d9-4b62-4949-85e4-2e6ea63041da.png)


크레딧이 자동으로 적용된다는 내용이었고, 대쉬보드를 확인해보니 요금이 거의 사라져 있었습니다.


![image](https://user-images.githubusercontent.com/80688900/176158338-77304c29-f4a0-4e98-b4bc-8551d5d5ea38.png)


휴... 마침 EC2 프리티어 경고 메일도 도착했길래 인스턴스 종료도 바로 진행했습니다. 조만간 다시 처음부터 워크샵을 진행한 후 Warehouse on Redshift도 포스팅할 예정입니다.