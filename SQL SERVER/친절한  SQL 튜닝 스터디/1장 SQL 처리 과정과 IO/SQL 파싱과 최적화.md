# 1장 SQL 처리 과정과 I/O



## SQL 파싱과 최적화



### SQL 최적화

SQL 실행하기 전 최적화 과정은 아래와 같다.



1. SQL 파싱

   사용자로부터 SQL을 전달받으면 가장 먼저 SQL 파서가 파싱을 진행한다.

   - 파싱 트리 생성 : SQL 문을 이루는 개별 구성요소를 분석해서 파싱 트리 생성
   - Syntax 체크 : 문법적 오류가 없는지 체크
   - Semantic 체크 : 의미상 오류가 없는지 체크 (존재하지 않는 테이블, 오브젝트 권한 등)

2. SQL 최적화

   옵티마이저가 다양한 실행경로를 생성 및 비교한 후 가장 효율적인 것을 선택한다.

   (데이터 베이스의 성능을 결정하는 가장 핵심)

3. 로우 소스 생성

   SQL 옵티마이저가 선택한 실행경로를 실제 실행 가능한 코드 또는 프로시저 형태로 포멧



### 옵티마이저 

사용자가 원하는 작업을 가장 효율적으로 수행할 수 있는 최적의 데이터 액세스 경로를 선택해 주는 DBMS의 핵심 엔진



- 옵티마이저의 동작 순서
  - 후보군이 될만한 실행계획 탐색
  - 데이터 딕셔너리 상에 미리 수집해둔 통계 및 정보를 이용해 예상비용 산정
  - 최저 비용을 나타내는 실행 계획을 선택



### 옵티마이저 힌트

옵티마이저의 선택이 항상 최선을 의미하지는 않는다. 이럴 때 옵티마이저 힌트를 통해 데이터 액세스 경로를 바꿀 수 있다.



```sql
#Oracle
SELECT /*+ INDEX(A 고객_PK) */
	   고객명, 연락처, 주소, 가입일시
  FROM 고객 AS A
 WHERE 고객 ID = '000000008'
 
#SQL SERVER
SELECT 고객명, 연락처, 주소, 가입일시
  FROM 고객 AS A WITH(INDEX(고객_PK))
 WHERE 고객 ID = '000000008'
```



옵티마이저 힌트의 경우 잘못된 실행 계획이 서비스에 큰 영향을 미칠 경우가 아닌 이상 항상 마지막 선택지점이 되어야하며 쓸 경우에는 옵티마이저가 다른 실행 계획을 탈 수 없도록 빈틈 없이 지정해야만 한다



SQL SERVER 힌트 목록

https://docs.microsoft.com/ko-kr/sql/t-sql/queries/hints-transact-sql-query?view=sql-server-ver15



