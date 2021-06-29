### Temp DB 증가시 확인 방법



시스템 데이터베이스 -> tempdb -> 보고서 -> 표준보고서 -> 상위 테이블의 디스크 사용

![image-20210629151643878](C:\Users\mkjung.KSYSTEM\AppData\Roaming\Typora\typora-user-images\image-20210629151643878.png)



아래와 같이 tempdb에서 상위 임시테이블의 사용량을 확인할 수 있다.

![image-20210629151551517](C:\Users\mkjung.KSYSTEM\AppData\Roaming\Typora\typora-user-images\image-20210629151551517.png)



또는 DMV 뷰를 통해 확인도 가능하다.

- sys.dm_db_file_space_usage :  각 파일에 대한 공간 사용량 정보를 반환
- sys.dm_db_session_space_usage : 각 세션에 의해 할당 및 할당 해제 된 페이지 수를 반환 (tempdb 에서만 적용)
- sys.dm_db_task_space_usage : 작업 별 페이지 할당 및 할당 해제를 반환

 

```sql
SELECT
  SUBSTRING(st.TEXT, dmv_er.statement_start_offset/2 + 1, (CASE WHEN dmv_er.statement_end_offset = -1 THEN LEN(CONVERT(NVARCHAR(MAX),st.TEXT)) * 2
                                                           ELSE dmv_er.statement_end_offset
                                                           END - dmv_er.statement_start_offset)/2) AS Query_Text,
  (dmv_tsu.user_objects_alloc_page_count - dmv_tsu.user_objects_dealloc_page_count) AS OutStanding_user_objects_page_counts,
  (dmv_tsu.internal_objects_alloc_page_count - dmv_tsu.internal_objects_dealloc_page_count) AS OutStanding_internal_objects_page_counts,
  dmv_er.start_time,
  dmv_er.open_transaction_count,
  dmv_er.reads,
  dmv_er.writes,
  dmv_er.logical_reads
FROM sys.dm_db_task_space_usage dmv_tsu
     INNER JOIN sys.dm_exec_requests dmv_er
       ON (dmv_tsu.session_id = dmv_er.session_id AND dmv_tsu.request_id = dmv_er.request_id)
     INNER JOIN sys.dm_exec_sessions dmv_es
       ON (dmv_tsu.session_id = dmv_es.session_id)
     CROSS APPLY sys.dm_exec_sql_text(dmv_er.sql_handle) st
WHERE (dmv_tsu.internal_objects_alloc_page_count + dmv_tsu.user_objects_alloc_page_count) > 0
```



SELECT SUM(user_object_reserved_page_count) AS [user object pages used],  
(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;



SELECT * FROM sys.dm_db_partition_stats WHERE object_id = -1286247274


SELECT  * FROM SYSOBJECTS WHERE NAME LIKE '%#B3556C96%'



https://mozi.tistory.com/353



https://blog.naver.com/spprince/222130545220
