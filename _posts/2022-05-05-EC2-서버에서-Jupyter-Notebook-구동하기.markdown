---
layout: post
title:  "Github Actions를 이용한 잔디심기 대쉬보드 만들기"
author: 김예나
date:   2022-05-03 23:20:00 +0900
categories: [study]
tags: [github actions]
math: true
mermaid: true
---


글을 본격적으로 시작하기에 앞서, 이 글은 <https://github.com/yonsei-app-dev-club/yonsei-app-dev-club-2022>의 코드를 그대로 사용하였음을 밝힙니다.


## 1\. 레포지토리 생성하고 권한 설정하기


잔디심기 대쉬보드를 만들 레포지토리를 생성합니다. 이미 있는 레포지토리에 표시하고 싶다면 굳이 생성하지 않아도 됩니다. 다만, workflow의 쓰기 권한이 허용되어 있는지 확인해 봅시다. 저는 이 권한 허용이 되지 않은 상태로 build를 진행해 에러가 발생했습니다.


![image](https://user-images.githubusercontent.com/80688900/166449024-e2b54b39-49be-4ca9-970e-a733cc692218.png)

(빌드 실패의 흔적들...)


Settings - Actions - General에 들어가면 다음과 같이 권한을 설정할 수 있습니다.


![image](https://user-images.githubusercontent.com/80688900/166455246-c7c05aae-0ffc-4fde-8896-11155a84ba7f.png)


## 2\. metrics fork하기


<https://github.com/lowlighter/metrics>


metrics는 잔디 뿐 아니라 most used languages, comment reactions 등 다양한 기능을 제공하는데 다음에 기회가 된다면 metrics를 이용해 다른 작업도 해보고 싶습니다.


![image](https://user-images.githubusercontent.com/80688900/166453963-3e56272d-0944-42b1-a3d0-5df742530fbc.png)


fork 버튼을 눌러 원하는 저장소에 metrics를 가져갑니다.


## 3\. token 생성하기


![image](https://user-images.githubusercontent.com/80688900/166455448-7f74c879-4963-47b7-8565-97796330783a.png)


Settings를 누르고 가장 아래의 메뉴인 Developer settings - Personal access tokens에 들어갑니다.


![image](https://user-images.githubusercontent.com/80688900/166455680-be8f362d-88f0-4ff0-8b5a-f3dd08676c1e.png)


Generate new token 버튼을 누르고 로그인해 다음과 같이 설정합니다.


![image](https://user-images.githubusercontent.com/80688900/166456264-321880bd-f3c6-44aa-a1de-ebf5da616b35.png)


note에는 토큰을 사용하는 목적을 적고 expiration에는 토큰의 유효기간을 정합니다. 저는 일단 90일로 두었는데, 원하는대로 설정하시면 됩니다.


![image](https://user-images.githubusercontent.com/80688900/166456776-8731927d-d803-4ce6-a2f8-996cd149cfdc.png)


Select scopes는 권한을 사용하는 만큼만 최소로 설정하는 것이 좋다고 쓰여 있기 때문에 저와 동일하게 체크하시고 generate token을 눌러 토큰을 생성해줍니다.


생성하고 나면 토큰을 복사할 수 있는 페이지가 나오는데, 두 번은 볼 수 없기 때문에 잘 복사해둡니다.


## 4\. Actions secrets에 토큰 등록하기


![image](https://user-images.githubusercontent.com/80688900/166457746-beb33f48-7239-4d90-bfd9-61ef8dc63adf.png)


아까 대쉬보드를 만들기로 했던 레포지토리에 들어가 Settings - Secrets - Actions에서 New repository secret 버튼을 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/166457877-2e574810-a6f6-49cf-b0de-0e2915969a4a.png)


name에는 secret의 이름을 적고 Value에 아까 복사해준 토큰을 집어넣고 add secret을 눌러 토큰을 등록합니다.


## 5\. .github/workflows에 yml 파일 만들기


![image](https://user-images.githubusercontent.com/80688900/166464567-546c00b0-5f2a-4252-911c-e41f70e9ac73.png)


actions 탭에서 파란 글씨로 되어있는 set ip a workflow yourself를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/166462552-f863ee7b-70a1-45f8-a816-5544be8c073b.png)


yml의 이름은 마음대로 설정해도 되지만 저는 코드를 따온 곳에서 사용한 파일명을 그대로 적었습니다. 내용은 다음과 같이 적습니다. 대괄호로 표시한 부분은 개인에 맞춰 설정해야 하는 부분입니다.


```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # 하루에 한 번 씩 빌드 수행
  schedule:
    - cron: '0 1 * * *'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - name: [빌드를 수행할 때 표시할 작업명]
        uses: [metrics를 fork한 저장소명]/metrics@latest
        with:
          token: ${{ secrets.[Actions secrets에 등록한 secret 이름] }}
          filename: [빌드한 후 생성할 파일명].svg
          base: ""
          plugin_isocalendar: yes
          plugin_isocalendar_duration: full-year
```


만약 여러 명을 대쉬보드에서 표시하고자 한다면 - name부터 plugin_isocalendar_duration까지를 반복해 적으면 됩니다.


## 6\. 시험 구동해 보기


다 적었으면 파일을 commit하고 다시 actions 탭으로 이동합니다. 이 파일은 점심(12시)에 새로고침 되기 때문에 그때까지 기다릴 수 없으니 확인하기 위해 시험 구동을 합니다.


![image](https://user-images.githubusercontent.com/80688900/166467576-baa6cf07-d8e5-4ad3-a381-fe86e04d09f0.png)


CI를 선택합니다.


![image](https://user-images.githubusercontent.com/80688900/166467726-48730955-31f9-4c1b-a64f-a0443f68a676.png)


Run workflow - Run workflow를 누릅니다.


![image](https://user-images.githubusercontent.com/80688900/166469651-c2f1c852-bbb1-40a9-94e0-c76c29ed889d.png)


![image](https://user-images.githubusercontent.com/80688900/166469869-2c62e2db-05ff-4f69-84be-979df2491f5c.png)



체크 표시가 되고 레포지토리에 파일이 무사히 생성되었다면 이제 마지막 단계만이 남았습니다!


## 7\. README.md 파일 생성 또는 수정하기


README 파일이 없다면 Add file - Create new file에서 README.md를 생성하고, 이미 있다면 수정해줍시다. 저는 이미 있었기 때문에 수정하겠습니다.


마크다운 문법을 이용해 원하는대로 본인을 소개한 뒤, Metrics 뒤에 /yml에서 설정한 filename을 입력합니다. 저는 다음과 같이 입력했습니다.


```markdown
# data-science-2022-biginner
2022년도 Data Science biginner 모임

## 참가 멤버


### ![김예나](https://avatars.githubusercontent.com/u/80688900?s=32&v=4) [김예나](https://github.com/ynkim0) - https://ynkim0.github.io/
[![Metrics](/github-metrics-ynkim0.svg)](https://github.com/ynkim0)
```


