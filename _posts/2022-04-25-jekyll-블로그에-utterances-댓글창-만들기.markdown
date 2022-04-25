---
layout: post
title:  "jekyll 블로그에 utterances 댓글창 만들기"
author: 김예나
date:   2022-04-25 21:00:00 +0900
categories: [study]
tags: [jekyll, utterances]
math: true
mermaid: true
---


블로그에 댓글창을 만들기 위해 사용할 수 있는 것들을 알아보니 disqus, utterances, giscus가 있었습니다. disqus는 부분 유료화와 오래 사용하면 광고가 붙는 등의 문제가 있다고 해서 패스하고, 당시에는 giscus의 존재를 몰랐기 때문에 utterances를 고르게 되었습니다. utterances는 github의 issues에 댓글을 등록시켜 관리하는 시스템입니다. giscus는 utterances + 답글 외 몇몇 기능을 가지고 있는데, 다음에 기회가 된다면 giscus를 적용해봐야겠습니다.


<https://utteranc.es/>


utterances 사이트에 들어가면 친절하게도 세 가지 주의사항을 이야기해줍니다. 레포지토리는 public일 것, 해당 레포지토리에 utterances app을 설치할 것, 레포지토리를 fork해서 가져왔다면 setting 탭에서 issue를 활성화할 것. 저는 첫 번째와 세 번째는 문제가 없었기 때문에 utterances app을 다운받는 것부터 시작해보겠습니다.


## 1\. utterances app 다운받기


<https://github.com/apps/utterances>


위 사이트에 들어가면 초록색 install 버튼이 뜨는데, 해당 버튼을 누른 후 All repositores 대신 Only select repositories를 선택하고 본인 블로그 저장소를 고릅니다. 이때 저장소는 public이어야 합니다. 선택을 모두 마치고 install을 누르면 utterances app이 설치됩니다.


## 2\. utterances 세팅하기


<https://utteranc.es/>


utterances 사이트에 들어갑시다. configuration 부분을 보면 됩니다.


![image](https://user-images.githubusercontent.com/80688900/165087914-ccae1ffc-f0a0-418d-a5cd-b4c72d6a6d4b.png)


입력하라는대로 본인 계정 이름과 repo 이름을 입력해 줍시다. 저의 경우에는 ynkim0/ynkim0.github.io가 되겠네요.


![image](https://user-images.githubusercontent.com/80688900/165088114-04305aed-afbb-4dbe-9ae1-9af9625e8293.png)


Blog 포스트와 Issue를 어떻게 연결할지를 정합니다. 저는 첫번째 설정으로 놔두었습니다.


![image](https://user-images.githubusercontent.com/80688900/165088407-fee82bff-af3e-4530-9b3c-3b99e51483ac.png)


utterances를 통해 생성된 issue를 구분하기 위해 어떤 label을 달지를 정합니다. 저는 어차피 issue를 댓글만을 위해 사용하기 때문에 비워두었습니다.


![image](https://user-images.githubusercontent.com/80688900/165089632-bf040e64-9a1c-482d-8f1a-42f0ebfd06da.png)


테마를 고릅니다. 개인적으로는 밝은 색의 깔끔한 느낌을 좋아해서 기본으로 설정되어있던 GitHub Light로 정했습니다.


![image](https://user-images.githubusercontent.com/80688900/165089833-a65b1b17-8e55-46c7-8053-e1bf057ef134.png)


세팅이 끝났습니다. Enalbe Utterances에 있는 코드를 Copy 버튼을 눌러 복사해둡시다.


## 3\. _layout/post 파일에 복사한 코드 넣기


댓글은 post에 붙어야 하기 때문에 _layout/post 파일을 열어 가장 아래에 복사한 코드를 넣고 저장해줍니다.


여기까지 완료한 후 로컬에서 수정한 내용을 push하면 블로그에 댓글 창이 추가됩니다.


![image](https://user-images.githubusercontent.com/80688900/165091322-dc24b1fb-e871-498a-bd14-2a961f7d8306.png)


이와 같은 형태이며, github 로그인을 하고 댓글을 작성하면 저장소의 issue에 댓글이 등록됩니다.