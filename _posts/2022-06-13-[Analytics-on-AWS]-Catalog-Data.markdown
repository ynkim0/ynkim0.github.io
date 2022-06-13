---
layout: post
title:  "[Analytics on AWS] Catalog Data"
author: 김예나
date:   2022-06-13 20:00:00 +0900
categories: [aws]
tags: [analytics on aws]
math: true
mermaid: true
---


이번 Catalog Data에서는 AWS Glue Data Catalog에 데이터 세트를 등록하여 Glue Crawlers의 도움으로 메타 데이터 캡처를 자동화 한다고 합니다. 카탈로그 엔터디가 생성되면 Amazon Athena에서 데이터의 raw 포맷의 데이터에 대해 쿼리를 시작할 수 있습니다.


![image](https://user-images.githubusercontent.com/80688900/173232550-14fae824-8032-43fe-8295-096684f13b3f.png)


## 1\. IAM Role 생성


IAM 콘솔 <https://us-east-1.console.aws.amazon.com/iamv2/home#/roles>로 이동하여 새 AWS Glue service role을 생성합시다.


![image](https://user-images.githubusercontent.com/80688900/173232651-7ace149d-191a-423c-9280-f70204a54012.png)


역할 만들기를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/173233310-b3ab7504-4973-43f2-99a9-6a2d7229530c.png)


엔터티 유형은 AWS 서비스, 사용 사례는 Glue를 선택합니다. 다음을 누르면 권한 추가 창이 나옵니다.


AmazonS3FullAccess를 검색하고 체크박스를 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/173311609-be7c293d-a403-4dbe-b530-1b9b31ccecdd.png)


선택했다면 필터를 지워주고, AWSGlueServiceRole를 검색합니다. 필터가 그대로 남아있다면 검색이 되지 않으니 반드시 필터를 지워주세요.


![image](https://user-images.githubusercontent.com/80688900/173312335-eed8a4a2-1d18-4419-ac56-bb7a96019d87.png)


이제 다음을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173312769-1a5a0990-9967-48f1-8b04-bc918c537717.png)


역할 이름은 AnalyticsworkshopGlueRole로 합니다.


![image](https://user-images.githubusercontent.com/80688900/173312952-9af35bff-d183-4ebb-a368-0bb2579dbef2.png)


권한에 AWSGlueServiceRole과 AmazonS3FullAccess만이 있는지 확인합시다. 두 개만이 있다면 역할 생성을 누릅니다.


## 2\. AWS Glue Crawlers 생성


AWS Glue 콘솔로 이동하여 Glue Crawlers를 생성하고 S3에서 새로 수집된 데잍의 스키마를 검색하는 단계입니다.


Glue 콘솔 <https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#/v2/home>로 이동합니다.


![image](https://user-images.githubusercontent.com/80688900/173315317-342ae3ad-99cf-48a3-98d2-e50955ade0f0.png)


왼쪽 패널에서 Crawlers를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/173315999-10725c7a-c695-4358-aba5-390e2e4db03a.png)


크롤러 추가를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173316618-78af735b-9ffd-411c-95d9-20c5faecc47e.png)


크롤러 정보를 누른 뒤 크롤러 이름에는 AnalyticsworkshopCrawler를 입력하고 다음을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173337018-6ce1e009-7cd7-4952-8fb2-38320fd89548.png)


위와 똑같이 설정하고 다음을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173343687-00a5186d-bd3c-4995-8a16-0025aea8e2d1.png)


데이토 스토어는 S3, 다음 위치의 데이터를 크롤링은 내 계정의 지정된 경로, 포함 경로는 s3://yourname-analytics-workshop-bucket/data/로 설정하고 다음을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173338914-91cdad95-967b-479c-ace3-84188e74a30e.png)


아니요를 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/173339536-eb6a2784-0502-46e0-832c-a04f37264e2b.png)


기존 IAM 역할 선택, Role 이름은 앞에서 만들었던 AnalyticsworkshopGlueRole을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/173340009-b5f0f417-d13f-427f-8149-1183c7cf821f.png)


빈도는 온디맨드 실행으로 합니다.


![image](https://user-images.githubusercontent.com/80688900/173340493-e4f27433-b1b1-493b-b225-f09d07a7425a.png)


데이터베이스 추가를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173340770-b6c7a9b4-7c29-41c5-8094-6e38bfc00a87.png)


데이터베이스 이름은 analyticsworkshopdb로 하고 생성을 누릅니다.


다음을 누르면 단계별 구성을 확인할 수 있는데, 잘 검토하고 마침을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173341644-67551286-c228-4340-b282-dfa296a28ce3.png)


AnalyticsworkshopCrawler의 체크박스를 선택하고 크롤러 실행을 누릅니다. 실행되는 동안 잠시 기다립니다.


## 3\. 카탈로그에서 새로 생성 된 테이블 확인


<https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=databases>로 이동하여 크롤링 된 데이터를 탐색합시다.


![image](https://user-images.githubusercontent.com/80688900/173342493-a1318751-0e65-4dae-8fa5-43841f38ec04.png)


analyticsworkshopdb를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/173342684-43e6a7a4-6bc0-4c56-b37c-fd7fc8d943c2.png)


analyticsworkshopdb 내 테이블을 클릭합니다.


raw에 들어가면 데이터세트의 스키마를 둘러볼 수 있습니다. averageRecordSize, recordCount, compressionType가 기록되어 있습니다.


## 4\. Amazon Athena를 사용하여 수집 된 데이터 쿼리


<https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#/query-editor>에 들어갑니다.


![image](https://user-images.githubusercontent.com/80688900/173345210-47619ddc-f7bd-4e34-ba79-3e860af7f16e.png)


위 캡처본처럼 첫 번째 쿼리를 실행하기 전에, Amazon S3에서 쿼리 결과 위치를 설정해야 합니다.라는 경고창이 뜬다면 보기 설정을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173345478-278b5d50-a1b9-4723-9d7b-87c92bae076b.png)


여기서는 관리를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/173345689-7437d106-e755-40be-9064-b58e35fcd8a6.png)


위처럼 쿼리 결과의 위치에 s3://yourname-analytics-workshop-bucket/query_results/를 입력하고 저장을 누릅니다.


문제를 해결했다면 다시 위의 링크로 들어가주고, 문제가 없다면 여기서부터 진행합니다.


![image](https://user-images.githubusercontent.com/80688900/173346878-67322e43-eb1e-4fff-ab8c-fc6293b1556e.png)


raw 옆에 점 세 개를 누르고 테이블 미리 보기를 눌러줍니다.


```sql
SELECT activity_type,
         count(activity_type)
FROM raw
GROUP BY  activity_type
ORDER BY  activity_type
```


위 코드를 복사한 뒤 다른 쿼리 편집기에 붙여넣고 실행 버튼을 눌러줍니다. 아래 사진은 이미 실행한 상태라 다시 실행이라고 표시되어 있습니다.


![image](https://user-images.githubusercontent.com/80688900/173347560-0b8f3d35-0b84-4ffb-8b50-9dcc0bfa3ad7.png)


카탈로그화된 결과는 다음과 같습니다.


![image](https://user-images.githubusercontent.com/80688900/173347814-0da48d49-aa54-4c07-8ae5-7d4d680586bc.png)


이제 데이터를 카탈로그화 했으므로 다음에는 AWS Glue ETL을 사용하여 데이터를 변환합니다.