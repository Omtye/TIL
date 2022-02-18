## AWS Aurora 시점 복원

DB 클러스터 특정 시점으로 복원

1. 복원하려는 DB 클러스터 선택 
2. 작업 > 특정 시점으로 복원
3. 사용자 지정 날짜 및 시간 선택 후 시점 입력

![Untitled 6](https://user-images.githubusercontent.com/43038052/154593866-9ba88b97-08d6-411f-a096-77bdb5d5f2a8.png)

4. DB 클러스터 정보 입력 후 특정 시점으로 복원

<br>

💡 여기서 복원의 경우 기존 DB클러스터를 복원하는 것이 아닌 해당 시점의 데이터로 새로운 클러스터를 생성하는 것임
  

<br>

**참고**

[DB cluster를 특정 시간으로 복원](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/USER_PIT.html)
