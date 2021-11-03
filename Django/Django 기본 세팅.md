1. requirements.txt 파일 생성

   각 패키지의 버전을 기록하여 Django 설치 시 동일한 환경을 유지하기 위함

![image](https://user-images.githubusercontent.com/43038052/138906651-ca5f7d72-6b66-4e02-89c8-d20c8c171fbd.png)



2. 장고 패키지 설치

   ```shell
   $pip install django 
   
   #위의 방식으로 설치해도 되지만 패키지 버전 관리를 위해 requirements.txt 파일을 이용한다
   $pip install -r requirements.txt
   ```

   

3. project 설치

   ```shell
   $django-admin startproject 프로젝트명 .
   
   # django 프로젝트 정상적으로 생성됬는지 확인
   $python manage.py runserver 8000
   ```

   

4. model 생성

   ```shell
   $python manage.py startapp user
   ```

   

5. settins.py app 추가

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'app' #4번에서 생성한 app추가
   ]
   
   ```

   
