### Aurora MySQL binlog 가져오기



```
mysqlbinlog 
--read-from-remote-server 
--host=test-cluster.cluster-cghtjgokrsze.ap-northeast-2.rds.amazonaws.com 
--port=3306 
--user lunadba 
--password 
--base64-output=decode-rows 
--verbose mysql-bin-changelog.004156 > mysql-bin-changelog.txt
```

