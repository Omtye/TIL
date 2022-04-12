

레디스 서버 실행

```
Docker run redis
```


작성한 Dockerfile로 이미지 빌드
```
Docker build omty/docker-compose-app ./ 
```
- `./` 는 현재 디렉토리

<br>

```
Docker run omty/docker-compose-app
```
실행 시 오류 발생
그 이유는 컨테이너 사이 별도의 설정 없이 접근이 불가하기에 Node.js 앱에서 레디스 서버 접근할 수 없음

![image](https://user-images.githubusercontent.com/43038052/162985792-087f21ef-20e5-474f-bb87-a4ba33ffef8e.png)

<br>

멀티 컨테이너 상황에서 쉽게 네트워크를 연결시켜주기위해 Docker Compose사용

<br>

docker-compose.yml 파일 생성
- yaml, yml 파일은 일반적으로 구성 파일 및 데이터가 저장되거나 전송되는 응용 프로그램에서 사용되고 XML, json 포맷 대신 사람이 읽기 쉬운 포맷으로 나타냄

<br>

**docker-compose.yml**

```
version: "3"
services:
  redis-server:
    image: "redis"
  node-app:
    build: .
    ports:
      - "5000:8080"
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
```
<br>

Docker-compose 실행 (docker-compose 파일 위치에서 실행)
```
docker-compose up -d --build
```
- `-d` 앱을 백그라운드로 실행 앱에 나오는 output을 표시하지 않음
- `--build` 옵션을 줄 경우 다시 빌드를 수행(수정했을 경우)



