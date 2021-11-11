1. awsebcli 설치

```shell
$pip install awsebcli
```



2. eb init
   - resion : `Asia Pacific(Seoul)`
   - application 생성
   - Python 사용여부 확인 : `Y`
   - Platform 사양 선택 
   - Cannot setup CodeCommit because there is no Source Control setup, continuing with initialization
      Do you want to set up SSH for your instances? `Y`
   - Key-pair 생성 : 명칭 입력후 엔터, 엔터



3. .ebignore 파일 생성

   ```
   venv/*
   .vscode/*
   .idea/*
   *.pyc
   
   # Elastic Beanstalk Files
   .elasticbeanstalk/*
   !.elasticbeanstalk/*.cfg.yml
   !.elasticbeanstalk/*.global.yml
   ```



4. setting.py 수정

   ```python
   DEBUG = False
   ALLOWED_HOSTS = ['*']
   ```

   

5. .ebextensions 폴더 생성

   - 00_프로젝트.conf 파일 생성

   ```
   option_settings:
       aws:elasticbeanstalk:container:python:
           WSGIPath: anonymous.wsgi:application
           NumProcesses: 2
           NumThreads: 15
   
   packages:
       yum:
           postgresql-devel: []
   
   container_commands:
       00_wsgipass:
           command: 'echo "WSGIPassAuthorization On" >> ../wsgi.conf'
   ```

   

6. eb create
   - Environment Name
   
   - DNS CNAME 
   
   - load balancer type : application
   
   - Spot Fleet(Auto Scaling)  : N
   
     

