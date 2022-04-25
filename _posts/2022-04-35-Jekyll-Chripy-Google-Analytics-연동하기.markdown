---
layout: post
title:  "Jekyll Chripy Google Analytics 연동하기"
author: 김예나
date:   2022-04-25 22:00:00 +0900
categories: [jekyll]
tags: [jekyll, ga4]
math: true
mermaid: true
---


다른 Jekyll 테마는 어떨지 잘 모르겠는데, Chripy 테마에서는 ga4 연동이 어렵지 않았습니다. Chripy demo 블로그에 들어가보면 설정하는 법이 나와 있습니다.


<https://chirpy.cotes.page/posts/enable-google-pv/>


## 1\. Google Analytics에 관리자 계정 만들기


<https://analytics.google.com/analytics/web/provision/#/provision>


Google Analytics에 접속합시다. 이전에 사용하던 것이 없으면 측정 시작이 뜨는데, 저는 전에 앱을 만들 때 연동해놓은 게 있어서 그 화면이 떴습니다. 이런 경우에는 왼쪽 네비게이션 바에서 하단에 있는 관리를 누르면 계정을 만들 수 있습니다.


![image](https://user-images.githubusercontent.com/80688900/165095147-ab941fee-5d3a-4d59-a723-126fb9a3c3a2.png)


계정 이름을 정해줍니다. 아무거나 입력해도 됩니다. 다 입력했으면 다음을 누릅시다.


![image](https://user-images.githubusercontent.com/80688900/165095594-1f95241a-50da-4426-83d1-5dd1457d992e.png)


속성 이름도 정해줍니다. 본인에게 표시되는 이름이기 때문에 본인이 알아볼 수 있게 만들어줍시다. 저는 그냥 간결하게 blog라고 적었습니다.

그 다음에는 시간대를 대한민국으로, 통화를 대한민국 원으로 바꿔줍니다.


![image](https://user-images.githubusercontent.com/80688900/165096138-8ed93dc1-1174-4d00-bdbd-6df5293ea7a2.png)


비즈니스를 위해서 하는 건 아니지만 정보를 입력하라니 해줍니다. 업종은 선택하지 않아도 되고, 나머지는 본인에게 해당하는 부분을 선택합니다. 그리고 약관에 동의하라는 내용이 나오는데 동의하고 나면 계정이 만들어집니다.


## 2\. 데이터 스트림 설정


![image](https://user-images.githubusercontent.com/80688900/165096710-8c586579-8ba1-4bc1-be87-fc7e9c61b763.png)


우리는 블로그에 연동할 것이기 때문에 웹을 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/165096980-41c0579e-ecee-4fd9-9712-d8f30e635ac5.png)


블로그 주소를 넣고 스트림 이름을 짓습니다. 향상된 측정의 설정 탭에서 원하는 기능을 추가하거나 사용하지 않을 기능을 해제합니다. 저는 체크되었던 대로 두었습니다.


![image](https://user-images.githubusercontent.com/80688900/165098442-b75c4ade-d7af-4d1f-8a0e-70e2c59af8be.png)


세부 정보에 뜨는 측정 ID를 복사합니다.


## 3\. _config.yml 수정


_config.yml에 들어가면 google analytics의 id란이 있습니다. id에 방금 복사한 측정 ID를 넣어줍시다.


이제 push하기만 하면 애널리틱스 홈페이지에서 블로그의 방문자 등 분석 내용을 확인할 수 있습니다.


만약 jekyll 테마가 ga4를 지원하지 않는다면, 아까 측정 ID를 확인했던 웹 스트림 세부정보에서 태그하기에 대한 안내 - 새로운 온페이지 태그 추가 - 전체 사이트 태그(gtag.js) 웹사이트 작성 도구 또는 CMS에서 호스팅하는 사이트를 사용하는 경우 이 태그 사용을 클릭합니다.


![image](https://user-images.githubusercontent.com/80688900/165100168-e975df83-227e-4ca8-8d20-bc59e7c518bd.png)


나온 코드를 복사해 _includes 폴더의 google-analytics에 넣습니다. 파일 명은 다를 수 있지만 analytics 관련 파일에 넣으면 되고, 관련 파일이 없다면 head에 넣어줍니다.