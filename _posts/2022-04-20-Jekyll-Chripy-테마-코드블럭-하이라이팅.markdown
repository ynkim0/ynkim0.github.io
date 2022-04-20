---
layout: post
title:  "Jekyll Chripy 테마 코드블럭 하이라이팅"
author: 김예나
date:   2022-04-20 21:00:00 +0900
categories: [jekyll]
tags: [jekyll, rouge]
math: true
mermaid: true
---


얼마 전 블로그에 글을 쓰는 것에 관해, 코드블럭을 하이라이팅하라는 조언을 듣고 그 방법을 이리저리 찾아보았는데, 이상하게도 분명 코드블럭에는 python이라고 명시가 되어있는데 하이라이팅은 되지 않고 있었다는 사실을 발견했습니다. 이것이 저 개인의 문제였는지, 아니면 애초에 커스텀을 위해 남겨진 부분인지는 모르겠지만 이런 이유로 오늘은 코드블럭을 하이라이팅하는 방법에 관해 포스팅을 해보겠습니다. 


## 1\. _config.yml 수정


![image](https://user-images.githubusercontent.com/80688900/164222996-553f441d-bb28-4810-922e-33df1ff7dea5.png)


이 테마에는 이미 Kramdown에 관한 부분이 적혀있기때문에 input: GFM만 입력해 주면 됩니다.


## 2\. Ruby에서 rouge 설치


ruby에서 본인 블로그의 로컬 저장소로 이동(cd [로컬 저장소 주소])하여 다음과 같이 입력합니다.


```ruby
gem install rouge
```

## 3\. 테마 파일 다운로드


```ruby
rougify help style
=> base16, base16.dark, base16.light, base16.monokai, base16.monokai.dark, base16.monokai.light, base16.solarized, base16.solarized.dark, base16.solarized.light, bw, colorful, github, gruvbox, gruvbox.dark, gruvbox.light, igorpro, magritte, molokai, monokai, monokai.sublime, pastie, thankful_eyes, tulip
```


rougify help style을 입력하면 고를 수 있는 테마가 출력됩니다. 저는 pastie 테마를 선택했습니다.


```ruby
rougify style [선택한 테마] > [원하는 경로/파일명].css
```


이 코드를 입력하면 지정한 경로에 css 파일이 생성됩니다. 내용을 복사해서 쓸 예정이기 때문에 css 파일은 어떤 디렉토리에 다운받으시든 문제가 없습니다. 이 파일을 열어서 내용을 복사해둡시다.


## 4\. 다운받은 파일 내용 복사하여 /assets/css/style.sass에 붙여넣기


해당 경로의 style.sass에 가면 append your custom style below라는 주석이 달려 있습니다. 그 아래에 복사한 내용을 붙여넣기 해주면 됩니다.


![image](https://user-images.githubusercontent.com/80688900/164223247-0cd0265f-8e46-44f1-9ec7-5f8cf0f9e374.png)