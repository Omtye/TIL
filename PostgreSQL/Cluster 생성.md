
MSSQL, MySQL InnoDB와 다르게 PostgreSQL은 PK 생성시 Cluster가 되지 않기 때문에 별도로 Cluster 옵션을 부여해야한다.



```
-- 이미 존재하는 인덱스에 클러스터 옵션 부여
ALTER TABLE table_name CLUSTER ON index_name;
```

Clustering 시 인덱스를 기준으로 테이블이 재정렬된다.




참고
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jasmin7727&logNo=220624447384
