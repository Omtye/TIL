우선 아래의 링크로 들어가 AWS 계정을 생성해줍니다.

https://aws.amazon.com/ko/



## AWS EC2인스턴스 생성



이 후 EC2 서비스를 선택해줍니다.

EC2가 안보이시는 분은 상태 검색창에 EC2를 검색하시면 됩니다.

![image-20210608221209992](https://user-images.githubusercontent.com/43038052/125166849-97267e80-e1d8-11eb-9a79-a2d617fd7a06.png)

<br>

AWS는 전세계에 데이터 센터가 있는데 현재 우리가 있는 곳과 제일 가까운 위치를 선택해줍니다.

![image-20210608221234120](https://user-images.githubusercontent.com/43038052/125166871-aad1e500-e1d8-11eb-9e58-ff1358600af0.png)

<br>





그 후에 인스턴스로 이동해서 인스턴스 시작을 클릭합니다.
![image-20210608221437043](https://user-images.githubusercontent.com/43038052/125166882-b45b4d00-e1d8-11eb-92cf-085f8dfb8f17.png)

<br>




첫번째로 각 운영체제에 맞는 인스턴스를 생성해야하는데

저희는 간단한 실습을 위한 서버를 구성할 것이기 때문에 프리티어  Ubuntu를 선택해줍니다.

이번 실습에서 우분투 버전은 크게 상관이 없기 때문에 둘중 하나를 선택하시면 됩니다.

![image-20210608221522247](https://user-images.githubusercontent.com/43038052/125166887-bcb38800-e1d8-11eb-8044-22da9aedf1b3.png)

<br>

인스턴스 유형도 마찬가지로 프리티어 선택 후 검토 및 시작을 클릭해줍니다.

![image-20210608221543280](https://user-images.githubusercontent.com/43038052/125166905-c89f4a00-e1d8-11eb-8cc2-45564d27018e.png)

<br>

마지막으로 인스턴스를 시작해줍니다.

보안 그룹의 경우 뒤에서 웹서버용 포트를 열 것이기 때문에 우선 스킵해줍니다.

![image-20210608221623207](https://user-images.githubusercontent.com/43038052/125166913-d228b200-e1d8-11eb-9f4d-19f14f227b09.png)

<br>

인스턴스를 시작할 경우 키 페어를 생성해야합니다. 키페어는 내 인스턴스에 접근할 수 있는 열쇠이고 노출되거나 잃어버리면 안되기 때문에 꼭 주의하셔야합니다.

또 키페어를 다운로드 받아야 인스턴스를 시작할 수 있고 키페어는 추후에 다시 다운로드 받을 수 없으니 꼭꼭!!!! 잘 보관하도록 합시다!!



![image-20210608221825792](https://user-images.githubusercontent.com/43038052/125166920-dbb21a00-e1d8-11eb-9c2c-d12234abe9bd.png)

<br>



이 후 pem 파일이 있는 위치로 이동한 후 .ssh 폴더를 생성해주고 pem 파일을 .ssh폴더에 넣어줍니다.

```bash
$ mkdir .ssh
$ mv django.pem .ssh/
```



그리고 pem 파일의 권한을 변경해줍니다.

```bash
$ chmod 400 .ssh/omty-app.pem
```







이렇게 AWS 기본 설정은 끝났으며 지금부터 AWS에 Django 프로젝트를 배포해보겠습니다.



실행중인 인스턴스를 선택한 후 연결을 클릭하면

![image-20210608222854812](https://user-images.githubusercontent.com/43038052/125166927-e4a2eb80-e1d8-11eb-87b3-7701001d0d75.png)

<br>

아래와 같이 친절하게도 어떻게 AWS EC2에 연결할 수 있는지 설명이 되어있습니다.

![image-20210608223015165](https://user-images.githubusercontent.com/43038052/125166935-ec629000-e1d8-11eb-99e4-c5812bb03d5e.png)

<br>

pem 파일의 위치로 이동한 후 예시의 명령어를 실행해주세요

```bash
$ ssh -i "omty-app.pem" ubuntu@ec2-54-180-151-169.ap-northeast-2.compute.amazonaws.com
```



실행하시면 아래 그림처럼 ubuntu에 해당 인스턴스로 연결이 됩니다.

![image-20210608223348486](https://user-images.githubusercontent.com/43038052/125166946-f4bacb00-e1d8-11eb-9d83-c76df9e9b75f.png)

<br>

AWS EC2 서버의 기본 설정을 위해 아래의 명령어를 차례대로 실행해줍니다.

```bash
#패키지 정보 업데이트
$ sudo apt-get update

#의존성 검사 및 업그레이드
$ sudo apt-get dist-upgrade

# pip3 설치
$ sudo apt-get install python3-pip
```





## AWS EC2서버에서 Git Clone



AWS EC2서버에 프로젝트를 빌드하기 위해서는 우선 전단계의 Django 프로젝트를 github에 올려야합니다.

먼저 repository를 만들고 아래의 명렁어를 실행해 줍니다.

이때  AWS가 아니라 Django 프로젝트 폴더 위치에서 실행해주어야합니다.



git을 초기화 시켜줍니다. 이때 폴더에 .git 폴더가 생성됩니다.

```bash
$ git init
```



git에 현재 폴더 전체를 담습니다.

```bash
$ git add .
```



origin 이라는 이름의 원격 저장소를 생성해줍니다.

```bash
$git remote add origin [repository 주소]
```



add . 명령어로 담은 파일들을 commit 합니다. 

```bash
$ git commit -m "firts django"
```



해당 원격 저장소로 commit한 내용을 업로드합니다.

```bash
$ git push origin master
```



이후에 이렇게 프로젝트가 업로드된 것을 확인할 수 있습니다.
![image-20210608225949795](https://user-images.githubusercontent.com/43038052/125166956-fe443300-e1d8-11eb-8eb9-e5d047c58073.png)

<br>


다시 AWS 서버로 돌아와서 git clone을 진행합니다. (ubuntu 로그인한 상태)



장고 프로젝트는 /srv 디렉토리에 업로드해줍니다. 이때 srv의 소유자를 ubuntu로 변경해주어야합니다.

srv 디텍토리는 서버를 위한 폴더로 다른 디텍토리에 비해 외부 사용자들이 쉽게 접근할 수 있습니다.

```bash
$ sudo chown -R ubuntu:ubuntu /srv/
```



```bash
$ cd /srv
$ git clone [레포지토리 주소]
```



실행하면 아래 처럼 정상적으로 정상적으로 클론된 것을 확인할 수 있습니다.

```bash
ubuntu@ip-172-31-2-210:/srv$ git clone https://github.com/Omtye/django-template.git
Cloning into 'django-template'...
remote: Enumerating objects: 28, done.
remote: Counting objects: 100% (28/28), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 28 (delta 2), reused 25 (delta 2), pack-reused 0
Unpacking objects: 100% (28/28), done.
```



ls명령어를 실행해주면 repository 이름과 동일한 폴더가 생성된 것을 확인할 수 있습니다.

```bash
$ ls
django-template
```



마지막으로 프로젝트 라이브러리 버전을 일치시키기 위해서 requirements의 버전을 설치합니다.

이때 requirements.txt의 파일은 srv/리파지토리/장고프로젝트 밑에 존재합니다.

```bash
$ pip3 install -r requirements.txt
```



혹시 오류가 발생할 경우 아래의 명령어를 추가로 실행해줍니다.

```bash
$ sudo python3 -m pip install -U pip
$ sudo python3 -m pip install -U setuptools
```



