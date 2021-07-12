## DB 트랜잭션 로그 꽉 찬 오류 해결



### 처리 방법

1. 오류가 발생하는 DB의 복구 모델을 단순으로 변경

   - DB의 우측버튼 -> 속성 -> 옵션 -> 복구모델 단순

   ![img](file:///C:/Users/MKJUNG~1.KSY/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)



2. 파일 -> 로그 부분의 크기를 축소(1024)

   *** 행 데이터는 건드리면 안됨 ***

   ![img](file:///C:/Users/MKJUNG~1.KSY/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)



3. 복구모델을 다시 전체로 변경
   - 복구 모델을 단순으로 할 경우 트랜잭션 백업 및 로그 관리가 되지 않으니 주의해야한다.