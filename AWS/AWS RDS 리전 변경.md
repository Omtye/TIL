### AWS RDS 리전 변경



AWS 리전 을 잘못 설정한 후 RDS를 구성하여 Django API 호출 성능이 느려진 것을 확인하고 리전 변경을 진행



1. AWS RDS > 스냅샷 > 시스템에서 자동으로 백업된 최신 스냅샷 선택 

   ![image](https://user-images.githubusercontent.com/43038052/148574408-ecb4f78a-b0d6-4b01-8511-3d8b84f9db35.png)

   

2. 작업 > 스냅샷 복사

   

3. 대상 리전 및 식별자 입력 후 스냅샷 생성

   ![image](https://user-images.githubusercontent.com/43038052/148577844-ec3e7ed1-5e9a-4cd6-b807-0dbd1f9451d6.png)



4. Seoul 리전으로 변경 > 스냅샷  > 작업 > 복원 > RDS 인스턴스 생성