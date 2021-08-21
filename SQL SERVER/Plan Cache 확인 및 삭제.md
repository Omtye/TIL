### Plan Cache 확인 및 삭제



사용자정의함수, 프로시저, 트리거 등은 생성 시점에 이름이 부여되며 컴파일한 상태로 딕셔너리에 저장되고 사용자가 삭제하지 않는 이상 영구히 보관된다. 또한 라이브러리 캐시에 적재되어 다수의 사용자가 공유하며 재사용할 수 있다.



일회성(Ad-hoc) 쿼리의 경우에도 각각 ID가 부여되고 공백하나의 차이도 각 SQL에 다른 아이디가 부여된다.  Adhoc 쿼리를 과도하게 사용할 경우 Query Plan에 의해 Plan Cache의 크기가 커지게 되며 사용 가능한 Buffer Pool 영역의 크기가 감소하여 SQL Server 성능이 저하될 수 있다.

```sql
-- Query Plan을 확인하는 쿼리
SELECT TOP 100
       cp.refcounts
      ,cp.usecounts
      ,cp.objtype
      ,st.dbid
      ,st.objectid
      ,st.text
      ,qp.query_plan
      ,cp.plan_handle
  FROM SYS.dm_exec_cached_plans AS cp
  CROSS APPLY SYS.dm_exec_sql_text(cp.plan_handle) AS st
  CROSS APPLY SYS.dm_exec_query_plan(cp.plan_handle) AS qp
```



아래 쿼리문을 사용하여 전체 또는 특정 쿼리 플랜을 제거할 수 있다.

```sql
-- plan_handle 값을 입력하여 특정 Query Plan을 제거
DBCC FREEPROCCACHE(plan_handle)

-- 전체 쿼리 플랜 제거
DBCC FREESYSTEMCACHE ('ALL','default');

-- Ad-hoc 쿼리 플랜 제거
DBCC FREESYSTEMCACHE('SQL Plans')

```



프로시저를 사용하여 데이터를 처리하는 경우 `DBCC FREESYSTEMCACHE('SQL Plans')` 명령을 통해 Ad-hoc 쿼리를 제거하는 것이 좋다. 전체 쿼리 플랜을 제거할 경우 모든 쿼리에 대해 실행계획을 재생성하여 많은 비용이 소요되기 때문에 일시적으로 성능이 저하될 수 있으니 주의해야한다.

