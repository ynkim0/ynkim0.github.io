---
layout: post
title:  "EC2 서버에서 Jupyter Notebook 구동하기"
author: 김예나
date:   2022-05-05 18:00:00 +0900
categories: [aws]
tags: [aws, ec2]
math: true
mermaid: true
---


<https://dataschool.com/data-modeling-101/running-jupyter-notebook-on-an-ec2-server/>의 내용을 따라해보며 EC2 서버에서 Jupyter Notebook을 구동해보았습니다.


## 1\. AWS 계정 만들기


<https://aws.amazon.com/>에서 계정을 생성합니다. 주소 입력, 이메일 인증, 카드 등록 등의 과정을 거치면 됩니다.


## 2\. EC2 Instance 만들기


가입을 완료한 화면에서 AWS Management Console로 이동할 수 있습니다.


![image](https://user-images.githubusercontent.com/80688900/166899606-31c5d2ae-1742-4b9d-97ac-cfbb2782bd3e.png)


이동한 후 모든 서비스 탭을 열어서 컴퓨팅의 첫 번째에 있는 EC2에 들어갑시다.


![image](https://user-images.githubusercontent.com/80688900/166899691-11a6068b-29fb-4958-92e8-3e3bca4c2584.png)


인스턴트 시작에 들어가면 가장 먼저 이름을 설정해야 하는데, 원하는대로 설정해주면 됩니다. 저는 web server test라고 적었습니다.


![image](https://user-images.githubusercontent.com/80688900/166900186-dd64ff02-4b18-446d-86f7-67405c4e33d1.png)


다음으로는 운영체제를 선택합니다. 저는 위 링크의 내용을 그대로 따라 실습했기 때문에 ubuntu를 선택했습니다.


![image](https://user-images.githubusercontent.com/80688900/166900334-4e8ec787-9335-4cc2-9b08-b91903f1cd52.png)


인스턴스 유형을 기본적으로 프리 티어 사용 가능한 t2.micro로 그냥 뒀습니다. 1년간 무료로 사용할 수 있다고 합니다.


![image](https://user-images.githubusercontent.com/80688900/166900649-4a9741fb-4101-41ba-b834-2d91c36fad9d.png)


이제 키 페어를 생성합니다.


![image](https://user-images.githubusercontent.com/80688900/166900776-77326e9d-c661-4eed-9755-04ae93f226c7.png)


새 키 페어 생성을 눌러줍니다.


![image](https://user-images.githubusercontent.com/80688900/166900856-1508badb-e149-4b0d-9304-adbb18ab4476.png)


이름을 입력하고 키 페어를 생성해주면 컴퓨터에 keypair.pem 파일이 다운로드되는데, 이 파일은 잃어버리면 안 되는 중요한 파일이므로 C드라이브에 디렉토리를 하나 만들어 넣어줍니다.


이 디렉토리로 들어가 keypair.pem의 속성을 열어줍니다. 보안 - 고급에서 다음과 같이 상속을 하지 않도록 왼쪽 아래 버튼을 눌러주고(누르면 뜨는 탭에서 위쪽 옵션 선택), 사용 권한에 User가 들어간 것들을 전부 제거해줍니다.


![image](https://user-images.githubusercontent.com/80688900/166904601-bcb672fe-b3d5-4fd9-b680-5a6e1f7a394a.png)


여기까지 완료했으면 나머지 부분은 설정대로 두고 쭉 아래로 내려 인스턴스 시작을 누릅니다.


## 3\. 보안 그룹 설정하기


네트워크 및 보안 - 보안 그룹에 들어가면 보안 그룹 생성 버튼이 있습니다. 눌러줍니다.


![image](https://user-images.githubusercontent.com/80688900/166903221-f9e7625f-ace8-4fd6-b35c-7cc539df62a7.png)


VPC는 자동으로 생성된대로 두고, 인바운드 규칙 3개를 만들어줄 겁니다. 규칙 추가를 3번 눌러준 뒤 다음과 같이 입력합니다.


![image](https://user-images.githubusercontent.com/80688900/166903495-b18336e8-8f58-4b3d-a6ca-fdc721d0eeed.png)


이제 스크롤을 아래로 내려 보안 그룹을 생성합니다.


## 4\. 보안 그룹 변경하기


인스턴스 - 인스턴스로 이동합니다.


![image](https://user-images.githubusercontent.com/80688900/166903686-c1fab44a-4521-41f4-93e4-0e604e148bfc.png)


위와 같이 인스턴스에 체크를 해 선택해주고, 작업 - 보안 - 보안 그룹 변경을 눌러줍니다.


연결된 보안 그룹에 아까 만들었던 Jupyter 보안 그룹을 넣습니다.


## 5\. EC2에 연결하기


인스턴스 - 인스턴스에서 체크를 해 선택하면 위에 있는 연결 버튼이 활성화됩니다. 연결을 누르고 SSH 클라이언트로 이동하면 다음과 같은 글을 볼 수 있습니다. 가장 마지막에 있는 코드를 복사합니다.


![image](https://user-images.githubusercontent.com/80688900/166905069-f6bcf3d1-9cee-463f-a650-68f110dc2a1e.png)


이제 터미널을 관리자 권한으로 실행하고 cd 명령어를 이용해 아까 keypair를 넣어둔 디렉토리로 이동합니다. 저는 C드라이브에 keypair 디렉토리 안에 넣어뒀으므로 다음과 같이 입력했습니다.


```shell
cd C:/keypair
```


다음으로는 아까 복사한 코드를 넣습니다.


```shell
ssh -i "keypair.pem" ubuntu@ec2-3-38-107-39.ap-northeast-2.compute.amazonaws.com
```


![image](https://user-images.githubusercontent.com/80688900/166934716-95c64d58-4565-4c12-964b-aaaa58886451.png)


이런 문구와 함께 다음 명령하는 부분이 ubuntu@ip로 바뀌었다면 여기까지 성공입니다.


## 6\. Jupyter Notebook 다운받고 path 설정하기


Jupyter Notebook 다운로드를 위해 다음 명령들을 차례로 입력해줍니다.


```shell
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
bash Anaconda3-2019.03-Linux-x86_64.sh
```


이어서 다음 명령들은 Jupyter Notebook의 path를 설정합니다.


```shell
export PATH=/home/ubuntu/anaconda3/bin:$PATH
source .bashrc
```


## 7\. Jupyter Notebook 설정하기


Jupyter configuration 파일을 만들어줍시다.


```shell
jupyter notebook --generate-config
```


이제 ipython을 이용해 Jupyter Notebook의 비밀번호를 만들어줄 겁니다.


```shell
ipython
```


여기까지 입력하고 나면 command가 python의 콘솔 창으로 바뀌게 됩니다. 참고한 문서에서는 from IPython.lib import passwd를 입력하라고 했는데 저는 계속 에러가 발생해서 from notebook.auth import passwd를 입력했습니다. 아마 passwd가 속한 패키지가 바뀌지 않았나 싶습니다.


```python
from notebook.auth import passwd
passwd()
```


차례로 입력하면 password를 두 번 쓰라고 하는데, 원하는 비밀번호로 써주시고 output으로 나오는 문자열을 복사해 두셔야 합니다. 관련 내용을 검색해보니 대부분 Sha1 암호가 나왔는데 저는 argon2가 나왔지만 그대로 복사해서 쓰니 문제 없이 작동했습니다.


이제 jupyter notebook에서 빠져나오기 위해 ctrl + z를 누릅니다. 그러면 다음과 같은 내용이 출력되면서 다시 ubuntu shell로 돌아갑니다.


![image](https://user-images.githubusercontent.com/80688900/166938099-f3ed3c3a-ffb7-4513-88e4-44fff291d87a.png)


이번에는 jupyter의 config 파일을 수정해줄겁니다. 명령 창에 다음과 같이 입력합니다.


```shell
cd .jupyter
vim jupyter_notebook_config.py_
```


그러면 파란 물결들이 뜨면서 파일을 읽는 창으로 전환됩니다. i를 누르면 아래에 [insert]라고 뜨면서 파일을 수정할 수 있습니다. 이 상태에서 ip, password, port를 입력해 줍니다.


```python
conf = get_config()

conf.NotebookApp.ip = '0.0.0.0'
conf.NotebookApp.password = u'YOUR PASSWORD HASH'
conf.NotebookApp.port = 8888
```


your password hash에는 아까 복사한 output를 그대로 가져다 쓰면 됩니다. 다 쓰고 나면 ESC를 누릅니다. ESC를 누르면 insert 모드에서 나올 수 있으며, 그 상태에서 :wq를 누르고 enter를 치면 다시 python 창으로 돌아옵니다. 이제 ctrl+z로 shell 창으로 돌아온 후 노트북을 저장할 디렉토리를 생성합시다.


```shell
mkdir MyNotebooks
```


MyNotebooks 외에 다른 이름으로 디렉토리를 생성해도 괜찮습니다.


## 8\. EC2 Jupyter Server에 연결하기


이제 Jupyter Notebook을 실행하면 EC2 서버에 접근할 수 있습니다. Shell 창에 jupyter notebook을 입력합니다.


```shell
jupyter notebook
```


서버가 열렸습니다. 인스턴스 - 인스턴스에 가서 해당 인스턴스에 체크하면 아래에 세부 정보가 표시됩니다. 그 중 퍼블릭 IPv4 주소를 복사하고 브라우저 주소창에 다음과 같이 입력합시다.


http://[퍼블릭 IPv4 주소]:8888


단, https를 사용하면 사이트에 보안 연결할 수 없다는 내용이 표시되면서 ERR_SSL_PROTOCOL_ERROR가 발생합니다. 저는 여기서 헤메다가 도움을 요청해 겨우 문제를 찾았는데, 이 에러가 발생하신 분들은 https를 사용 중이 아닌지 확인해 보시면 될 것 같습니다.


마지막으로, 이 사이트에 접속하면 password 또는 token을 입력하라고 하는데 passord는 여러분이 위에서 입력하신 password이고, token은 shell 창에 token 뒤에 써있는 부분을 따오면 됩니다.