---
layout: post
title:  "Jekyll 테마를 적용한 github 블로그 만들기"
author: 김예나
date:   2022-04-16 17:00:00 +0900
categories: [git]
tags: [git, jekyll]
math: true
mermaid: true
---
  

로컬에서 성공적으로 페이지를 띄우고, 배포만을 남겨둔 상태에서 첫 글을 썼었는데 그때로부터 지금까지, 장장 20시간 가량에 걸쳐 문제를 해결하며 드디어 블로그를 열었습니다. 허탈하게도 고민한 시간에 비해 너무나도 단순한 방법으로 해결되었기에 이 글에서는 github.io 블로그를 생성하는 법과 겪었던 문제들, 그리고 해결 방법에 관해 다루어 보겠습니다.


## 1\. Ruby 다운로드


Ruby : <https://www.ruby-lang.org/ko/documentation/installation/>

이 페이지에 루비 설치 방법에 관해 자세히 나와 있습니다. 저는 ruby에 관해서는 잘 몰랐기 때문에 windows를 위한 <https://rubyinstaller.org/>에서 다운받아 설치하였습니다.


## 2\. 깃헙 레포지토리 만들고 clone하기


github-page를 구성할 레포지토리를 만듭니다. 레포지토리 이름은 반드시 username.github.io로 합니다.

![KakaoTalk_20220415_212204133](https://user-images.githubusercontent.com/80688900/163570207-b54ae389-e39b-4049-94ce-4a92a0cd5f02.png)

로컬에 레포지토리를 clone하는 법을 모르신다면 <https://ynkim0.github.io/posts/%EB%A1%9C%EC%BB%AC%EC%97%90%EC%84%9C-github-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%ED%91%B8%EC%8B%9C%ED%95%98%EA%B8%B0/>을 참고해주세요.


## 3\. 원하는 테마 고르고 로컬 저장소에 파일들 옮기기


jekyll 테마를 찾아볼 수 있는 사이트는 많습니다. 그 중 하나를 소개하겠습니다.

<https://jamstackthemes.dev>

저는 깔끔하고, 프로필을 왼쪽에서 볼 수 있는 테마를 원했는데 생각보다 그런 테마를 찾기는 쉽지 않았습니다. 저와 취향이 같은 분들을 위해 두 개의 테마를 추천하겠습니다.


PlainWhite
![screenshot](https://user-images.githubusercontent.com/80688900/163569242-6ba32843-ee47-4e73-9ead-7bcce972318f.png)
<https://github.com/samarsault/plainwhite-jekyll>


Chirpy
![KakaoTalk_20220415_210713906](https://user-images.githubusercontent.com/80688900/163569173-c402b7f4-cb0c-4f8e-8d3b-b07cbc3a1446.png)
<https://github.com/cotes2020/jekyll-theme-chirpy>
  

개인적인 취향으로는 PlainWhite가 더 마음에 들었는데, 계속해서 github의 build error가 발생하는 바람에 적용하지 못하고 Chripy를 적용하였습니다.

![KakaoTalk_20220415_211823187](https://user-images.githubusercontent.com/80688900/163569831-7cb01b2b-9fc8-4eba-8b9f-278cec481593.png)
원하는 테마를 찾은 후, github에 들어가서 zip 파일로 다운받습니다.


다운 받은 zip파일의 압축을 해제하고, 내부 파일을 전부 선택해 복사하고 Clone한 파일에 붙여 넣으면 이 단계는 끝입니다.



## 4\. initiallize 하기


git bash를 열어 로컬 저장소가 있는 디렉토리로 이동(cd 저장소 경로)한 뒤 다음과 같이 입력합니다.

```bash
tools/init.sh
```
프로필 사진이나 post 등을 삭제해 사용자가 직접 지우지 않아도 되도록 초기화해줍니다.



## 5\. _config.yml 수정하기


저는 이 부분에서 어디부터 어디까지 수정해야 할지 감이 안 왔는데, 혹시나 저와 같은 분들을 위해 수정해야 할 부분을 안내합니다.

lang: ko

timezone: Asia/Seoul

title: 블로그 타이틀

tagline: 타이틀 아래 설명

description: >-                        

  블로그 소개

url: 'https://username.github.io/'

github:

  username: 깃헙 username

social:

  name: 본명

  email: 이메일 주소

  links: 
    - 본인이 가지고 있는 sns 링크

theme_mode:  light 또는 dark 선택하여 입력

avatar: 'avatar 이미지가 있는 주소'


개인적인 생각으로는 title과 tagline은 짧게 입력하는 편이 깔끔해 보이는 것 같습니다.


이 설정에서 애먹었던 부분은 avatar에 사용할 이미지를 assets의 img 파일에 집어넣고 해당 로컬 주소(/assets/img/profie.jpg)를 입력했는데 이미지가 적용되지 않았습니다. 당시에는 initiallize를 건너뛰고 했기 때문에 문제가 발생했는지는 모르겠지만 그래서 저는 github의 프로필 이미지 주소를 따서 넣었고, 그렇게 하고 나니 잘 동작했습니다.
![image](https://user-images.githubusercontent.com/80688900/163667180-c8d96f34-ee25-485b-b2bf-51ff9ba21169.png)


## 6\. 로컬에서 확인하기


![image](https://user-images.githubusercontent.com/80688900/163667447-dc5b3ecc-8af9-4466-8575-c685c8e8fb45.png)


Ruby Command를 열어 로컬 저장소가 있는 디렉토리로 이동(cd 저장소 경로)한 뒤 다음과 같이 입력합니다.

여기까지 완료되었으면 다음 명령들을 순서대로 입력합니다.

```ruby
gem install jekyll bundler
bundle
jekyll serve
```

저는 jekyll serve를 입력했을 때 webrick이 없다고 error가 발생했는데, 만약 이 문제가 발생했다면 bundle add webrick을 입력해 webrick을 추가해주면 해결됩니다.

정상적으로 서버가 열렸다면 Server address: 뒤에 서버 주소가 출력됩니다. 해당 주소를 복사해서 브라우저에 붙여넣기 하면 로컬에서 블로그 창을 확인해볼 수 있습니다. 더 수정하고 싶은 내용이 있다면 서버를 닫지 않고도 수정해줄 수 있는데, post를 추가하거나, 여타 설정을 바꿀 수 있습니다. 다른 부분은 수정 후 브라우저 창을 새로고침하면 잘 반영되는데 반해 _config.yml을 수정하면 서버를 닫았다가 다시 열어야 반영되었습니다.


## 7\. Gemfile.lock 문제 해결하기


Gemfile.lock이 github에 push되면 문제를 일으킵니다. 왜 문제가 일어나는지는 알 수 없으나, 저의 경우 build가 실패하기도 하고, 페이지가 정상적으로 출력되지 않고 index 파일만 출력되기도 하는 등 어쨌거나 로컬에서는 아무리 해봐도 문제 없이 동작하는데 github에 올렸을 때 무언가 잘못되었다면, 이 파일을 확인해볼 필요가 있겠습니다.

해결법은 다음과 같습니다.

(1) .gitignore의 # bundler cache 아래에 Gemfile.lock 추가하기

(2) Gemfile.lock 파일 삭제하기

저의 경우 (1)만 하고 (2)를 하지 않았을 때 앞서 언급했던 페이지가 정상적으로 출력되지 않고 index 파일만 출력되는 문제가 발생했습니다. 둘 다 하지 않았을 때는 Action 탭에서 Error: The process '/opt/hostedtoolcache/Ruby/2.7.6/x64/bin/bundle' failed with exit code 16과 같은 에러가 발생하였음을 확인했습니다.


plainwhite 테마를 적용했을 때는 빌드 중에 Error:  The plainwhite theme could not be found가 발생했는데, 이 문제는 해결법을 찾지 못했습니다. 분명 plainwhite는 멀쩡히 업로드되어 있었는데 왜 인식하지 못하는지 모르겠습니다. 구글링을 하다가 같은 문제가 발생했다는 글을 두 개정도 발견했지만 해결책은 쓰여 있지 않았습니다. 다만, 이때는 Gemfile.lock 파일을 삭제하지 않은 채로 올렸었기 때문에 혹시나 삭제해보면 어땠을까 하는 생각이 듭니다.


## 8\. github에 push하고 branch 세팅 바꾸기


Ruby Command를 열어 로컬 저장소가 있는 디렉토리로 이동(cd 저장소 경로)한 뒤 다음과 같이 입력합니다. Ruby 외의 다른 command창을 사용해도 괜찮습니다.

```shell
git add .
git commit -m "blog"
git push -u origin main
```

여기까지 하면 저장소에 gh-pages라는 브랜치가 생겨 있습니다. 저는 7번의 문제를 해결하지 않아 잘 구동되었다는 체크표시가 뜸에도 불구하고 이 브랜치가 생기지 않았었는데, 혹여나 gh-pages가 생성되지 않았다면 무언가 문제가 발생한 것이므로 무슨 문제가 있는지 한 번 더 검토해보시기 바랍니다.


정상적으로 생성되었다면 저장소의 Settings - Pages - Source를 찾아 Branch를 Main에서 gh-pages로 변경하고 Save를 눌러 줍니다. 이제 빌드가 완료되었다는 체크 표시가 뜨면 username.github.io에 들어가 페이지가 잘 출력되는지 확인해봅시다.


