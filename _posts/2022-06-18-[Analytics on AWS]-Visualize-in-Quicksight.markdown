---
layout: post
title:  "[Analytics on AWS] Visualize in Quicksight"
author: 김예나
date:   2022-06-18 22:00:00 +0900
categories: [aws]
tags: [analytics on aws]
math: true
mermaid: true
---


이번에는 Amazon Quicksight를 사용하여 S3에 수집, 저장된 데이터에 대해 몇 가지 시각화를 구축할 것입니다.


![image](https://user-images.githubusercontent.com/80688900/174439262-b7c03028-252c-4bf3-b4c9-a5931589a4e6.png)


## 1\. QuickSight 셋팅


이 단계에서는 QuickSight를 사용하여 processsed data를 시각화합니다.


먼저 Quicksight 콘솔 <https://us-east-1.quicksight.aws.amazon.com/sn/start>로 이동합니다. AWS에 로그인이 되어 있고 Quicksight 계정이 없는 상태라면 Sign up for QuickSight 버튼을 볼 수 있습니다. 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174439358-821b2473-8fd5-4c19-88ab-bc1db02b8212.png)


Enterprise를 선택하고 Continue 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/174439435-8fb73e10-862d-4a37-9957-f4b44eb51140.png)


나머지는 기본 설정 그대로 두고 account name은 yournameanalyticsworkshop로, Notification email address는 본인의 이메일을 입력합니다.


![image](https://user-images.githubusercontent.com/80688900/174439498-15c9c639-fae8-4770-b3a1-a0c3f101ab95.png)


이 부분에서는 Amazon S3와 Amazon Athena만 체크해 줍니다. Amazon S3에서는 yourname-analytics-workshop-bucket만 선택합니다. 여기까지 끝났으면 Finish를 클릭합니다.


## 2\. 새로운 데이터세트 추가


<https://us-east-1.quicksight.aws.amazon.com/sn/start/data-sets>로 이동합니다.


![image](https://user-images.githubusercontent.com/80688900/174439664-e8aec520-fe8a-46af-b3bb-1b5924a92288.png)


Datasets 탭으로 이동한 후 New dataset을 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174439694-eb084ad9-10e9-4501-9bba-3be16af54736.png)


Athena를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174439719-8a27b381-280a-4a3a-8a48-3692f5425d6b.png)


Data source name은 analyticsworkshop으로 하고, Validate connection을 클릭하여 위와 같이 Validated 상태로 만들어준 후 Create data source 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/174439834-ec2d5e51-f414-4bdf-a195-fad87f62a5c3.png)


위와 동일하게 설정하고 Select를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174439864-572764d3-c049-4423-afc2-18c5127f0300.png)


Directly quert your data를 선택하고 Visualize를 누릅니다.


## 3\. Amazon Quicksight를 사용하여 processsed data 시각화


### 시각화 1 : 사용자가 듣고 있는 트랙의 히트맵


여기서는 어떤 사용자가 반복적으로 트랙을 듣고 있는지 보여주는 시각화를 합니다.


![image](https://user-images.githubusercontent.com/80688900/174440189-37a8f7e8-ccdf-4269-ae6d-4fc1af704f13.png)


왼쪽 하단의 Visual types에서 Heat Map을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/174440253-94fb5cf7-87d2-44b5-8311-14c7adaa0cc3.png)


왼쪽 Fields list에서 device_id를 선택하고 track_name을 선택합니다. 위쪽에 있는 Field wells에 Rows: device_id, Columns: track_name가 표시되었다면 잘 설정된 것입니다.


![image](https://user-images.githubusercontent.com/80688900/174440311-45b3ca4d-a414-4524-b2fb-1e03f32e1bc4.png)


이처럼 진한 파란색 패치 위로 마우스를 가져가면 특정 사용자가 동일한 트랙을 반복적으로 듣고 있음을 알 수 있습니다.


### 시각화 2 : 가장 많이 연주 된 아티스트 이름의 트리맵


이 단계에서는 가장 많이 플레이 된 아티스트를 보여주는 시각화를 만듭니다.


![image](https://user-images.githubusercontent.com/80688900/174440393-24a4e546-d717-483b-8b76-a0f2b35327b3.png)


왼쪽 상단 + Add 클릭 후에 Add Visual를 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174440588-c45b72e3-2cb0-4025-af76-e283675c5814.png)


Visual types는 Tree Map을 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/174441115-95e15006-18d1-463c-8751-f30cbc8094ad.png)


Fields list에서 artist_name을 선택하면 가장 많이 플레이 된 아티스트를 시각적으로 확인할 수 있습니다.