## SQL SEVER 트랜잭션 격리 수준



### 격리 수준 설정

```sql
SET TRANSACTION ISOLATION LEVEL
    { READ UNCOMMITTED
    | READ COMMITTED
    | REPEATABLE READ
    | SNAPSHOT
    | SERIALIZABLE
    }
```

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

<br>

**READ UNCOMMITTED**

- 다른 트랜잭션에 의해 수정되었지만 커밋되지 않은 행을 읽을 수 있음 (잠금 X)
- S Lock 실행 안함, X Lock 이 걸려있어도 읽어 올 수 있음
- 모든 SELECT 문 테이블에 NOLOCK을 설정하는 것과 같은 기능

<br>

**READ COMMITTED**

- 다른 트랙잭션에 의해 수정되었지만 커밋되지 않은 데이터를 읽을 수 없도록 함
- READ UNCOMMITTED 와 달리 SELECT 할때` S Lock을 소유`하고 SELECT가 끝나면 `S Lock을 해제`

- SELECT가 끝나더라도(커밋되기전) 다른 세션에서 INSERT나 UPDATE가 가능

<br>  

**REPEATABLE READ**

- 다른 트랜잭션에 의해 수정되었지만 아직 커밋되지 않은 데이터를 읽을 수 없도록 지정하고 현재 트랜잭션이 완료될 때까지 현재 트랜잭션이 읽은 데이터를 다른 트랜잭션이 수정할 수 없음

- 처음 읽었을 당시에 존재하는 데이터만 Lock을 걸고 현재 행이 읽고 있는 데이터 외는 추가가 가능

<br>  

**SERIALIZABLE**

- 인덱스를 기준으로 해당 이전 행, 다음 행까지의 Range-S-S Lock을 건다.
- 인덱스가 없을 경우 범위 파악이 되지 않아 전체 행 Lock

<br>  

이 부분은 조금 더 상세한 예시로 블로그에 정리해보자
