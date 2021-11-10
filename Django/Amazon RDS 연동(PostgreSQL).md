

1. 데이터베이스 생성

![image](https://user-images.githubusercontent.com/43038052/140954747-3c1d9c6d-1530-4ead-a604-d74bd40884bb.png)



2.데이터베이스 구성 설정

![image](https://user-images.githubusercontent.com/43038052/140956493-d2e1eb4d-a020-400c-a2e3-ab9e681defd4.png)



3. DB 정보 입력

   ![image](https://user-images.githubusercontent.com/43038052/140956669-717f605a-cb63-44b5-86db-1d98d28a74f8.png)



4. 보안 그룹 규칙 

   - Inbound 설정

     ![image](https://user-images.githubusercontent.com/43038052/140958836-fd96f0e5-4a80-4c46-b57f-42aec89543c3.png)

   

   - 인바운드 규칙 편집

     ![image](https://user-images.githubusercontent.com/43038052/140959143-1b5a3619-e695-403f-a94a-7e44c8673451.png)

     ![image](https://user-images.githubusercontent.com/43038052/140959063-99c673ef-e5bf-4e0e-bb51-cc24ec1097aa.png)



5. 퍼블릭 엑세스 활성화

   - RDS 수정 클릭

   ![image](https://user-images.githubusercontent.com/43038052/140959965-5fe9d7b9-8a73-4d20-b627-f928ea3b599d.png)

   

   - 퍼블릭 액세스 가능으로 변경 (즉시 적용 선택)

     ![image](https://user-images.githubusercontent.com/43038052/140960329-cc1f1839-5e00-407a-b502-1d47315a9372.png)



6. DBeaver에서 접속 테스트

   ![image](https://user-images.githubusercontent.com/43038052/140961407-bd213804-4b09-40a1-a473-fcd74873432e.png)

   - Host : AWS 엔드포인트
   - Database : postgres
   - Username : 위에서 생성한 마스터 사용자
   - Password : 패스워드

7. 신규 데이터베이스 생성

   ![image](https://user-images.githubusercontent.com/43038052/140962460-1b523228-c2dc-40e9-910d-c0e3b7ad6f83.png)

8. env.json 파일 생성

   ```json
   "DATABASE_NAME":"데이터베이스명칭",
   "DATABASE_HOST":"AWS 엔드포인트",
   "DATABASE_USER_NAME":"계정명",
   "DATABASE_PASSWORD":"패스워드"
   ```



9. settings.py Database 설정 변경

   ```python
   import json
   
   env_json = 'env.json'
   with open(env_json) as f:
       env_json = json.loads(f.read())
   
   DATABASES = {
        'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': env_json['DATABASE_NAME'],  #Database 명칭
           'USER': env_json['DATABASE_USER_NAME'],   #사용자명
           'PASSWORD': env_json['DATABASE_PASSWORD'],   
           'HOST': env_json['DATABASE_HOST'],  
           'PORT': '5432',       #PostgreSQL 기본 포트
       }
   }
   ```



10. migrate 실행