---
layout: post
title:  "Analyze with Athena"
author: 김예나
date:   2022-06-17 21:00:00 +0900
categories: [study]
tags: [machine learning]
math: true
mermaid: true
---


저번 시간까지 Glue와 Glue Studio를 이용해 데이터를 변환하는 방법을 알아보았는데, AWS Glue DataBrew와 EMR 부분은 생각보다 이번 달 비용 발생이 커 우선 패스하고 Analyze with Athena부터 진행해보려 합니다.


![image](https://user-images.githubusercontent.com/80688900/174300437-8617b270-dc85-4cc5-9e0f-5ad73e6b9856.png)


이 단계에서는 Amazon Athena를 사용하여 변환 된 데이터를 분석합니다.


우선 Amazon Athena 콘솔 <https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#/query-editor/history/fe17d747-ca38-4e3b-80df-4ee2276fa2bf>에 들어갑니다.


![image](https://user-images.githubusercontent.com/80688900/174302503-37664de5-b741-4de9-a26a-9eaf0bfe27d5.png)


만약 첫 번째 쿼리를 실행하기 전에, Amazon S3에서 쿼리 결과 위치를 설정해야 합니다와 같은 메세지가 표시되었다면 보기 설정을 누릅니다.


해당 화면은 그대로 두고, S3 콘솔 창 <https://s3.console.aws.amazon.com/s3/get-started?region=us-east-1>을 엽니다.


![image](https://user-images.githubusercontent.com/80688900/174303129-cfe068bf-d738-4ecc-9ec8-92d4083d60e4.png)


버킷 만들기를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/174303289-163f48f5-64e7-4ffe-a2dc-d3c637be231d.png)


위와 같이 버킷명은 yourname-query-results로 설정한 뒤 아래로 스크롤해 버킷 만들기를 누릅니다.


버킷을 생성했다면 아까 켜뒀던 Athena 설정 창으로 넘어갑니다.


![image](https://user-images.githubusercontent.com/80688900/174303488-e8c6193d-41e6-4326-8d39-8639a1d67eb0.png)


관리를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/174303604-357bcdcb-9322-4fec-83d9-a3020ed94fda.png)


쿼리 결과의 위치를 s3://yourname-query-results/로 설정해준 후 저장을 누릅니다.


이제 다시 Athena 콘솔 <https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#/query-editor/history/fe17d747-ca38-4e3b-80df-4ee2276fa2bf>에 들어갑니다.


Athena는 데이터 원본을 추적하기 위해 AWS Glue 카탈로그를 사용하므로 Glue의 모든 S3 지원 테이블이 Athena에 표시된다고 합니다.


![image](https://user-images.githubusercontent.com/80688900/174304147-93281ea9-c6bf-4d67-8b02-54ec7cb31e8c.png)


데이터베이스를 analyticsworkshopdb로 설절한 상태에서 쿼리를 만들어 다음 코드를 복사해서 붙여넣기한 후 실행합니다.


```sql
SELECT artist_name,
       count(artist_name) AS count
FROM processed_data
GROUP BY artist_name
ORDER BY count desc
```


![image](https://user-images.githubusercontent.com/80688900/174304398-f3ead0d6-0720-49f6-813c-6ee2193f6ef2.png)


결과로 아티스트 이름이 Count 순으로 나왔습니다.


저는 emr 실습을 진행하지 않았지만 만약 진행했다면 emr_processed_data 테이블을 이용하여 장치에서 반복적으로 재생되는 트랙 목록을 반환하는 쿼리를 만들 수도 있습니다.