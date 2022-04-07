### Docker 명령어

<br>

| 명령어            |  설명                   |
| ----------------- | -------------------- |
| docker run <이미지 이름>  |                      |
| docker ps         | 현재 실행중인 컨테이너 |
| docker ps -a      |   모든 컨테이너 조회   |
| docker create <이미지 이름> | 이미지 파일 스냅샷 디스크에 올림 |
| docker start -a <시작할 컨테이너 이름/ID> | 컨테이너 프로세스에 올림 |
| docker stop <컨테이너 ID> | 컨테이너 중지 :: `SIGTERM` 호출 후 Grace Period(정리 시간)을 부여. 이후 `SIGKILL` |
| docker kill <컨테이너 ID> | 컨테이너 중지 :: 정리하는 시간없이 `SIGKILL` 호출 |
| docker rm $(docker ps -a -q) | 전체 컨테이너 삭제(실행중인 컨테이너는 삭제 X)(CMD에서는 안됨) |
| docker rmi <이미지 ID> | 이미지삭제 |
| docker system prune | 도커를 쓰지 않을 떄 모두 정리 (실행중인 컨테이너는 영향x) |
| docker exec [-it] <컨테이너 ID> <명령어> | 실행중인 컨테이너에 명령어 전달 | 
| docker exec [-it] <컨테이너 ID> sh | 해당 컨테이너 터미널 연결 | \
| docker volume rm <볼륭명> | 특정 볼륨 삭제 |
| docker volume prune | 사용하지 않는 볼륨 삭제 | 

