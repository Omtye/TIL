### Temp DB 증가 여부 확인

서버에서 갑자기 Database 파일 용량이 급격하게 증가하는 현상이 발생하는데 대부분의 경우 Temp DB의 용량이 비정상적으로 증가하여 발생하게 된다. 쿼리 실행시 While 문이나 임시테이블에 대량의 데이터를 Insert 하면서 발생하게 되는데 이를 해결하기 위해서는 Temp DB 축소 및 쿼리 최적화가 진행되어야 한다.

<br>

아래 이미지를 보면 Tempd Database 파일 용량이 약 350기가로 급격하게 증가

![image](https://user-images.githubusercontent.com/43038052/129386772-ad8a573a-e818-4302-8ec6-3a1a44aad759.png)

<br>

전체 Tempdb의 용량은 `sp_helpdb tempdb` 명령어로 확인이 가능하다.

![image](https://user-images.githubusercontent.com/43038052/129387210-2d526873-9662-4c6a-8019-9511928386e1.png)

<br>

시스템 데이터베이스 -> tempdb -> 보고서 -> 표준보고서 -> 상위 테이블의 디스크 사용 항목에서 

아래와 같이 tempdb에서 상위 임시테이블의 사용량을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/43038052/129387458-837a2952-3da2-44cd-967d-24054a0923ff.png)

<br>

또는 DMV 뷰를 통해 확인도 가능하다.

- sys.dm_db_file_space_usage :  해당 DB의 각 파일에 대한 공간 사용량 정보를 반환 
- sys.dm_db_session_space_usage : 각 세션에 의해 할당 및 할당 해제 된 페이지 수를 반환 (tempdb 에서만 적용)
- sys.dm_db_task_space_usage : 작업 별 페이지 할당 및 할당 해제를 반환

<br>

```sql
-- 현재 Tempdb를 사용하고 있는 쿼리를 탐색
SELECT SUBSTRING(st.TEXT, dmv_er.statement_start_offset/2 + 1, (CASE WHEN            dmv_er.statement_end_offset = -1 THEN LEN(CONVERT(NVARCHAR(MAX),st.TEXT)) * 2
                                                           ELSE dmv_er.statement_end_offset
                                                           END - dmv_er.statement_start_offset)/2) AS Query_Text
       ,(dmv_tsu.user_objects_alloc_page_count - dmv_tsu.user_objects_dealloc_page_count)           AS OutStanding_user_objects_page_counts
       ,(dmv_tsu.internal_objects_alloc_page_count - dmv_tsu.internal_objects_dealloc_page_count)   AS OutStanding_internal_objects_page_counts
       ,dmv_er.start_time
       ,dmv_er.open_transaction_count
       ,dmv_er.reads
       ,dmv_er.writes
       ,dmv_er.logical_reads
FROM sys.dm_db_task_space_usage AS dmv_tsu
JOIN sys.dm_exec_requests       AS dmv_er  ON dmv_tsu.session_id = dmv_er.session_id 
                                          AND dmv_tsu.request_id = dmv_er.request_id
JOIN sys.dm_exec_sessions       AS dmv_es  ON dmv_tsu.session_id = dmv_es.session_id
CROSS APPLY sys.dm_exec_sql_text(dmv_er.sql_handle) st
WHERE (dmv_tsu.internal_objects_alloc_page_count + dmv_tsu.user_objects_alloc_page_count) > 0

```

<br>

```sql
-- tempdb에서 사용자 개체에 의해 사용되는 전체 페이지 수와 공간(MB)을 반환합니다.
SELECT SUM(user_object_reserved_page_count)          AS [user object pages used],  
      (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;
```

