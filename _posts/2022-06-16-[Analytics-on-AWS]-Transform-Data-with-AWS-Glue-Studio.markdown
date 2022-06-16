---
layout: post
title:  "[Analytics on AWS] Transform Data with AWS Glue Studio"
author: 김예나
date:   2022-06-16 18:00:00 +0900
categories: [aws]
tags: [analytics on aws]
math: true
mermaid: true
---


AWS Glue Studio는 AWS Glue에서 추출, 변환 및 로드(ETL) 작업을 쉽게 생성, 실행 및 모니터링 할 수 있는 새로운 그래픽 인터페이스 입니다. 데이터 변환 워크플로우를 시각적으로 구성하고 AWS Glue의 Apache Spark 기반 서버리스 ETL 엔진에서 원활하게 실행할 수 있습니다.


이 실습에서는 Transform Data with AWS Glue 와 동일한 ETL 프로세스를 수행합니다. 하지만 이번에는 AWS Glue Studio의 시각적 그래픽 인터페이스를 활용합니다.


![image](https://user-images.githubusercontent.com/80688900/174034931-3c8b77eb-aafc-43df-99eb-30da692c6a2c.png)


먼저, Glue Studio 콘솔 <https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/>로 이동합니다.


![image](https://user-images.githubusercontent.com/80688900/174036309-00ef71bc-6122-4d74-9437-3995dbae9b60.png)


왼쪽 위 선 세 개를 눌러 메뉴를 확장한 후 Jobs를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174037089-b068fe22-c584-4335-a9fe-1b66e7f074ca.png)


Visual with a blank canvas를 선택하고 Create 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/174037360-3d049c32-4a5b-44b4-bb7b-27ef927f4c42.png)


Source 클릭 후 Amazon S3을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174037574-5658d0f2-6d68-42e3-9b11-1c2ed7f8f43e.png)


위와 동일하게 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174038624-00357c33-ad82-474f-b47c-08c68f5b43be.png)


다시 Source 클릭 후 Amazon S3을 선택하고, raw만 reference_data로 바꾸어 추가합니다.


![image](https://user-images.githubusercontent.com/80688900/174038976-d4d926f8-bf04-423a-bee2-7476593b1311.png)


두 개 중 임의로 한 개를 클릭한 후 transform - join을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174039403-6cfd53be-e9aa-4277-90c2-b25b0aafc877.png)


Transform에서 Node properties로 넘어갑니다.


![image](https://user-images.githubusercontent.com/80688900/174039847-ca019c54-4a75-4966-941b-04a8b3dcc816.png)


Node Parents에서 나머지 한 개도 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174040154-ae6d058f-7c27-459e-9ca1-d8673a85e8f9.png)


Transform 탭으로 넘어가 Add condition을 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174040506-e0ad73c2-fcdc-4e72-b23c-321dcb875e3a.png)


양쪽 모두 track_id를 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174040644-4e280bbf-0fc1-4766-9c52-51cbd7d1bf72.png)


이제 원래 선택되어 있던 대로 두고(Join 노드 선택) Tramsform을 클릭하고 ApplyMapping을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174041273-7738fe37-82dd-45d0-8e93-7af62ca50636.png)


위와 같이 .track_id, parition_0, parition_1, parition_2, parition_3은 drop하고 track_id는 string으로 변경합니다.


![image](https://user-images.githubusercontent.com/80688900/174041761-1269e43a-cb9f-4817-ae5e-c6cf46212c88.png)


Target 클릭 후 S3를 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174042483-01361619-c3d4-4e57-93f9-d15ac32a7828.png)


Data target properties - S3에 위과 같이 입력합니다. Format은 Parquet, Compression Type은 Snappy, S3 Target Location은 s3://yourname-analytics-workshop-bucket/data/processed-data2/(yourname은 본인 이름으로 변경), Data Catalog update options는 Create a table in the Data Catalog and on subsequent runs, update the schema and add new partitions를 선택하고 Database는 analyticsworkshopdb, Table name은 processed-data2로 합니다.


![image](https://user-images.githubusercontent.com/80688900/174043111-56d8ccdc-a88a-4acb-96e5-b6cce7d5f1b1.png)


상단 Job details를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174043703-67781f86-e574-4b52-980e-64ff396ff178.png)


Name은 AnalyticsOnAWS-GlueStudio, IAM Role은 AnalyticsWorkshopGlueRole로 합니다.


![image](https://user-images.githubusercontent.com/80688900/174043917-1a282ab6-7539-4d6b-8446-b3348e3469e7.png)


Number of workers는 2, Job bookmark는 Disable, Number of retries는 1, Job timeout는 10으로 설정하고 오른쪽 위에 있는 Save를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174061590-ad213af3-e129-482a-b715-630db4676297.png)


그런데 왜,,,Error가 떴는지 모르겠습니다. 다시 처음부터 해봐도 결과는 마찬가지... 해결하고 나서 추후 업데이트 하겠습니다.