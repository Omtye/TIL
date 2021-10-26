### 1. PostgreSQL 설치

https://www.postgresql.org/

<br>

### 2. DBeaver 설치

https://dbeaver.io/download/

<br>

### 3. Database 생성

DBeaver에 접속한 후 Database 생성

![image](https://user-images.githubusercontent.com/43038052/138903887-ee9ed043-2881-408d-b9a8-0e697f164789.png)

<br>

### 4. Database 설정 변경

settings.py 파일의 DATABASES 경로 변경

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'anonymous',  #Database 명칭
        'USER': 'postgres',   #사용자명
        'PASSWORD': '1234',   
        'HOST': 'localhost',  
        'PORT': '5432',       #PostgreSQL 기본 포트
    }

}
```

<br>

### 5. psycopg2 패키지 설치
``` shell
pip install psycopg2
```

<br>

### 6. 생성된 모델 테이블 생성

```shell
python manage.py makemigrations

python manage.py migrate
```





