### Django Project 구성

- ProjectName
  - settings.py : 각종 전역 설정(데이터베이스, 인증, 권한, 보안, 언더 등등)
  - urls.py : 전역 URL 설정
  - wsgi.py: Web Server Gateway Interface (웹 서버의 진입점)
- manage.py : Django 관리 스크립트
- templates : Templage 폴더 (html)
- static : 정적 파일 폴더 (img, js, css 등등)
- AppName 
  - migrations : migratio 폴더(DB Table 관리 파일)
  - models.py : 모델 (DB, Table 정의)
  - views.py : 로직