---
layout: post
title:  "[Analytics on AWS] Ingest and Store"
author: 김예나
date:   2022-06-10 14:00:00 +0900
categories: [aws]
tags: [analytics on aws]
math: true
mermaid: true
---


Analytics on AWS 워크샵에서는 분석 플랫폼을 구축하는 다양한 모듈 중 일부를 살펴보며 AWS Glue, Amazon Athena, Amazon EMR, Amazon QuickSight, AWS Lambda 및 Amazon Redshift와 같은 여러 분석 서비스를 사용하여 데이터를 수집, 저장, 변환, 소비하는 방법을 배웁니다.


![image](https://user-images.githubusercontent.com/80688900/172995362-cf57c48f-da6a-4a29-8873-9066d356fbaa.png)


이 워크샵의 학습 결과는 다음과 같습니다.


- 서버리스 데이터 레이크 아키텍처 설계
- Amazon S3를 스토리지를 사용하여 데이터를 Data Lake로 수집하는 데이터 처리 파이프라인 구축
- 실시간 스트리밍 데이터에 Amazon Kinesis 사용
- AWS Glue를 사용하여 데이터세트 자동 분류
- AWS Glue 개발 엔드포인트에 연결된 Amazon SageMaker Jupyter 노트북에서 대화형 ETL 스크립트 실행
- EMR을 사용하여 Spark 변환 작업 실행
- Glue에서 Amazon Redshift로 데이터 적재
- Amazon Redshift 모범 설계 사례 소개
- Amazon Athena를 사용하여 데이터를 쿼리하고 Amazon QuickSight를 사용하여 시각화


이 글은 [Analytics on AWS]의 워크샵 페이지를 보고 실습하였음을 밝힙니다.


Lab Guide를 보면 이 실습을 위한 전제 조건이 있습니다.


- AWS 계정에서 AdminstratorAccess에 대한 액세스 권한이 있어야합니다.
- 이 실습은 us-east-1 리전에서 실행되어야 합니다.
- 이 가이드의 링크에 따라 새 탭에서 여는 것이 가장 좋습니다.
- 최신 브라우저에서 이 실습을 실행하세요.


또한, 시작하기 전에 해야 할 일과 하지 말아야 할 일을 제시합니다.


- 모듈 내에서 학습을 시작하기 전에 각 모듈의 전제 조건을 확인하십시오.
- 새 브라우저 탭에서 모든 하이퍼 링크를 여는 것을 잊지 마십시오.
- 사용 가능한 모든 구성 옵션을 자유롭게 탐색하되 리소스 생성을 위해 실습 가이드에 언급 된 구성을 따르십시오. :)
- 또한 워크샵이 끝나면 리소스를 정리하는 것을 잊지 마십시오!


## 1\. S3 버킷 생성


우선 <https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1>로 이동하여 AWS 콘솔에 로그인합니다.


로그인을 마쳤다면, <https://s3.console.aws.amazon.com/s3/buckets?region=us-east-1>로 이동합니다.


![image](https://user-images.githubusercontent.com/80688900/172997292-7c89c692-fa27-4b87-87cc-330d1ca85fbb.png)


주황색 버킷 만들기 버튼을 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/172997559-24a82377-94a7-41c6-ae58-e601542807f9.png)


버킷 이름만 yourname-analytics-workshop-bucket으로 설정해주고 버킷 만들기를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/172997775-ecc4265b-fec6-4bc6-96a9-67f127c36352.png)


생성된 버킷의 이름을 눌러 버킷에 들어가줍니다.


![image](https://user-images.githubusercontent.com/80688900/172997927-71be7697-6d36-4915-8e24-05a77f87ce2a.png)


객체의 폴더 만들기 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/172998017-1d632a8f-96e3-4c72-b43a-763b176bc7dd.png)


폴더 이름을 data로 하고 폴더를 만듭니다.


![image](https://user-images.githubusercontent.com/80688900/172998110-634ace5f-4e38-4290-95f9-32addc08ee09.png)


이제 다시 파란색 이름의 data를 눌러 폴더 안으로 들어갑니다. 같은 방법으로 폴더 만들기를 누르고 reference_data라는 폴더를 만들어줍니다. reference_data라는 파란 글씨를 눌러 풀더 안까지 진입해 줍시다.


[tracks_list.json] 링크에 들어가면 json 파일이 보이는데, 이때 오른쪽 마우스 클릭을 하면 파일을 저장할 수 있습니다.


![image](https://user-images.githubusercontent.com/80688900/172998722-3342ef25-6487-4b85-921c-304ca9c4bf83.png)


tracks_list.json이라는 이름으로 저장해줍니다.


다시 reference_data 폴더로 돌아갑시다.


![image](https://user-images.githubusercontent.com/80688900/172998826-d8561774-8448-4db3-9afe-eecf682d4038.png)


업로드 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/172999001-7e46225d-237b-4bba-aeb1-937ba28d2adc.png)


파일 추가 버튼을 눌러 다운 받은 파일을 올린 뒤 업로드 버튼을 누릅니다.


## 2\. Kinesis Firehose 생성


Kinesis Firehose Console로 이동합시다.


![image](https://user-images.githubusercontent.com/80688900/172999368-ecd9938f-4692-4036-a755-89f678920a7b.png)


우측에 있는 전송 스트림 생성을 클릭합니다.


아마 워크샵 문서가 작성된 후 전송 스트림을 만드는 순서가 조금 바뀐 것 같지만 잘 찾아보면 문제 없이 설정할 수 있었습니다. yourname에 본인 이름을 넣는 것을 제외하고 설정은 다음 사진과 동일하게 합니다.


![image](https://user-images.githubusercontent.com/80688900/173000606-35424b07-90f7-41ca-838c-57f9efd0f16c.png)


![image](https://user-images.githubusercontent.com/80688900/173000657-09d1ca66-e662-4a1f-bcab-a1939640056b.png)


![image](https://user-images.githubusercontent.com/80688900/173000801-97e3c346-aa94-4014-8002-ac2d0bf315a3.png)


![image](https://user-images.githubusercontent.com/80688900/173000849-394902f3-2f28-4581-a5cc-612b668b8baa.png)


![image](https://user-images.githubusercontent.com/80688900/173000886-6239c478-471d-4062-894b-d81931b2239f.png)


여기까지 설정했으면 Create delivery stream을 클릭해 전송 스트림을 생성합니다.


## 3\. Dummy 데이터 생성


<https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Kinesis-Data-Generator-Cognito-User&templateURL=https://aws-kdg-tools-us-east-1.s3.amazonaws.com/cognito-setup.json>에서 스택을 생성합니다. 들어가자 마자 보이는 스택 생성 페이지에서 다음을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173001651-048717c3-d8b4-4ebe-8b54-b6cc6676cd2a.png)


Username에는 admin을 입력하고 Password에 비밀번호를 설정합니다. 이후 로그인에 사용해야 하므로 잘 기억해 둡시다.


다음을 누르고, 나온 페이지에서는 설정할 내용이 없으므로 또 다음을 한 번 더 눌러 줍니다.


스택 생성 전에 마지막으로 리뷰하는 페이지가 나오는데, AWS Cloud Formation에서 IAM 리소스를 생성할 수 있음을 승인한다는 내용에 체크를 해주고 스택 생성 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173002362-0342487f-f1c8-4128-9030-378309235fd5.png)


이제 AWS Cloudformation 콘솔을 새로고침하며 스택이 잘 생성될 때까지 기다려줍니다.


![image](https://user-images.githubusercontent.com/80688900/173002770-dbced70a-60c1-40f6-86be-0b4808449d20.png)


이렇게 초록색으로 CREATE_COMPLETE가 뜨면 Kinesis-Data-Generator-Cognito-User를 눌러 들어가 줍니다.


![image](https://user-images.githubusercontent.com/80688900/173002991-2cf41f04-1b57-42a4-baf6-8503900a20bc.png)


출력 탭에서 키 옆의 파란색 링크로 들어가 줍니다.


![image](https://user-images.githubusercontent.com/80688900/173003122-a90d2b85-5c59-4a48-a576-931bdc833e91.png)


아까 설정했던 비밀번호를 쓸 차례입니다. Username을 입력하는 칸에는 admin을, Password를 입력하는 칸에는 아까 설정한 비밀번호를 입력하고 로그인합니다.


![image](https://user-images.githubusercontent.com/80688900/173006747-2116674f-7957-4b15-b38e-0baec9813a61.png)


로그인하면 나오는 페이지에서 위와 같이 설정하고 코드를 복사해와야 합니다. 중괄호를 사용하면 liquid error가 발생해 글에 넣지는 못했고, <https://catalog.us-east-1.prod.workshops.aws/workshops/44c91c21-a6a4-4b56-bd95-56bd443aa449/ko-KR/lab-guide/ingest>의 하단에 코드블럭이 있습니다.


![image](https://user-images.githubusercontent.com/80688900/173006938-adddcfe8-1177-4d60-a893-d7f950a71d4b.png)


복사한 코드를 Record template (Template 1)에 붙여넣고 Send Data를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/173007097-68eb5bfe-be24-4612-a341-862d80f1c955.png)


총 10000개의 데이터가 전송되면 Stop sending data to Kinesis를 누릅니다.


## 4\. 데이터가 S3에 저장 되었는지 확인


<https://s3.console.aws.amazon.com/s3/home?region=us-east-1>에서 username-analytics-workshop-bucket => data => raw로 이동하면 firehose가 yyyy/mm/dd/hh 파티셔닝을 사용하여 데이터를 S3로 덤프 했다는 것을 알 수 있습니다.


[Analytics on AWS] :https://catalog.us-east-1.prod.workshops.aws/workshops/44c91c21-a6a4-4b56-bd95-56bd443aa449/ko-KR
[tracks_list.json] :https://static.us-east-1.prod.workshops.aws/public/b4ab4ec2-9ab1-4c41-bf19-964db5dcc495/static/data/tracks_list.json