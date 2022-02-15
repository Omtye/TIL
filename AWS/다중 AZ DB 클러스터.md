# AWS 장애 대응 정리

상태: 진행 중
완료예정일: 2022년 2월 18일
우선순위: 상
일자: 2022년 2월 14일

## 0. 다중 AZ DB 클러스터

다중 AZ DB 클러스터에서는 Amazon RDS가 DB엔진의 네이티브 복제 기능을 사용하여 라이터 DB의 인스턴스를 리더 DB 인스턴스로 복제한다. 변경 사항을 커밋하고 적용하려면 하나 이상의 리더 DB 인스턴스의 승인을 받아야한다. 

리더 DB 인스턴스의 역할

- 자동 장애 조치 대상
- 읽기 트래픽을 제공하여 애플리케이션 읽기 처리량을 높힘

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled.png)

### 0.1 다중 AZ 클러스터 엔드포인트

**클러스터 엔드포인트**

클러스터 엔드포인트(라이터 엔드포인트) 는 해당 DB 클러스터의 라이터 DB 인스턴스에 연결된다. DDL 및 DML 문과 같은 쓰기 작업이 가능하며 읽기 작업 또한 가능하다.

각 다중 AZ DB 클러스터에는 클러스터 엔드포인트와 라이터 DB 인스턴스가 하나씩 존재한다.

클러스터 엔드포인트는 DB 클러스터에 대한 읽기/쓰기 연결 시 장애 조치를 지원한다. 현재 DB 라이터 인스턴스에 장애가 발생하면 다중 AZ DB 클러스터가 자동으로 새 라이터 DB 인스턴스로 장애 조치를 수행한다. 

장애 조치가 이루어지는 동안에도 DB 클러스터가 새로운 라이터 DB 인스턴스의 클러스터 엔드포인트 연결 요청을 처리하여 서비스 중단 시간을 최소화한다.

[mydbcluster.cluster-123456789012.us-east-1.rds.amazonaws.com](http://mydbcluster.cluster-123456789012.us-east-1.rds.amazonaws.com/)

**리더 엔드포인트**

다중 AZ DB 클러스터의 리더 엔드포인트는 DB 클러스터에 대한 읽기 전용 연결을 위한 로드 밸런싱을 지원한다. 리더 DB 인스턴스에서 읽기 작업을 처리하여 라이터 DB 인스턴스의 오버헤드를 줄일 수 있다. 

리더 엔드포인트를 사용하는 세션의 경우 SELECT 작업만 사용할 수 있다.

[mydbcluster.cluster-](http://mydbcluster.cluster-ro-123456789012.us-east-1.rds.amazonaws.com/)[**ro**](mydbcluster.cluster-ro-123456789012.us-east-1.rds.amazonaws.com)[-123456789012.us-east-1.rds.amazonaws.com](http://mydbcluster.cluster-ro-123456789012.us-east-1.rds.amazonaws.com/)

**인스턴스 엔드포인트**

인스턴스 엔드포인트는 다중 AZ DB 클러스터에 있는 특정 DB 인스턴스에 연결된다. 

### 0.2 다중 AZ DB 엔드포인트가 고가용성으로 작동하는 방법

고가용성이 중요한 다중 AZ DB 클러스터의 경우 라이터 엔드포인트를 읽기/쓰기 등 범용 연결에 사용하고 리더 엔드포인트를 전용 연결에 사용한다. 

DB 클러스터의 기본 DB 인스턴스에 장애가 발생하면 Amazon RDS가 자동으로 새로운 라이터 DB 인스턴스로 장애 조치를 실행한다. **이 때 리더 DB 인스턴스를 새로운 라이터 DB 인스턴스로 승격하여 이를 수행한다.** 장애 조치가 발생하면 기존 라이터 엔드포인트를 사용하여 새롭게 승격된 라이터 DB 인스턴스에 연결이 가능하다. 또는 리더 엔드포인트를 사용하여 DB 클러스터에서 리더 DB 인스턴스 중 하나에 다시 연결 가능하다. 

순서 정리

라이터 장애 → 기존 리더가 라이터로 변경 → (잠시 동안 리더 엔드포인트가 새 라이터 DB 인스턴스에 연결 가능) → 기존 라이터의 작업 완료 후 리더 DB 인스턴스에 적용 → Failover 완료

## 1. AWS Aurora Failover(Switch)

AWS DB 클러스터의 경우 리더, 라이터 인스턴스로 나눠진다. 이 때 장애로 인해 라이터 DB 인스턴스에 중단이 발생하면 AWS 는 자동으로 다른 가용 영역에 있는 리더 DB 인스턴스가 라이터로 전환된다. 장애 조치 시간은 조건에 따라 다르나 일반적으로 20~40초 정도 소요된다. 

Failover는 기존의 라이터 인스턴스에서 장애로 인해 처리되지 않은 트랜잭션을 처리한 후 리더 DB 인스턴스에 적용되면 완료된다. (트랜잭션이 클 경우 시간이 더 지연될 수 있다)

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%201.png)

### 1.1 Failover 발생

때마침(?) 장애 상황이 발생하였다.

slack을 통해 ro 엔드포인트(리더 DB 인스턴스) 에 연결된 서비스에서 Connection 오류가 발생하여 지속적으로 알람이 발생하였다.

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%202.png)

Failover를 통해 기존 리더 인스턴스가 라이터로 승격되는 과정에서 Connection 문제가 발생하였고 Failover가 처리된 후에 알람이 중지된 것이 확인되었다.

**혹시 reader 인스턴스가 Failover 된 것은 아닐까? 하는 마음에 찾아본 결과 Aurora MySQL 2.10 이상의 버전에서는 라이터 DB 인스턴스와 장애 조치 대상만 다시 시작한다고한다.**

리더 인스턴스에 대해 장애가 발생하였을 경우 AWS 리더 엔드포인트가 Available 상태에 있는 다른 리더 인스턴스로 쿼리를 전달함. 

AWS 클러스터에 사용 가능한 리더 인스턴스가 없는 경우 RDS는 이 조건이 

**기존** 

admin-01 → 라이터 인스턴스

admin-02 → 리더 인스턴스

**Failover 후**

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%203.png)

**이벤트로그**

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%204.png)

### 1.2 Faliover 수동 처리 방법

문제가 발생한 DB 인스턴스를 선택 > 작업 > 장애조치(Failover) 를 선택한다.

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%205.png)

### 1.3 원인

현재 명확한 오류 이벤트 없이 Failover 가 발생했을 경우 아래의 이유 중 하나일 수 있다고한다.

- 가용 영역 장애
- 기본 DB 인스턴스 컴퓨팅 노드 장애
- 기본 DB 인스턴스 네트워킹 문제
- 스토리지 또는 Amazon Elastic Block Store(Amazon EBS) 볼륨 문제

### 1.4 참고

[다중 AZ DB 클러스터 배포(미리 보기)](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/multi-az-db-clusters-concepts.html#multi-az-db-clusters-concepts-failover-automatic)

[FailoverDBCluster](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_FailoverDBCluster.html)

[다중 AZ DB 인스턴스 배포](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html#Concepts.MultiAZ.Failover)

[Amazon Aurora의 고가용성](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)

[High availability for Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)

## 2. Aurota RDS 무중단 Scale-Up

Aurora에서 무중단으로 Scale-up 할 경우가 필요할 수 있다. 이러한 경우 읽기 인스턴스에 대해 Scale-up을 진행합니다. 이후 Failover를 통해 라이터 인스턴스로 승격시킨다. (기존의 라이터 인스턴스는 리더로 변경됨). 이 때 tier 우선 순위에 따라 기존 인스턴스가 자동으로 primary 로 복구될 수 있으니 조심해야한다.

**순서** 

1. Scale-up 대상 RDS의 Reader 인스턴스 Scale-up
2. Failover를 통해 Writer와 Reader를 교체한다. 
    
    (이 때 tier 우선 순위에 따라 기존 인스턴스가 자동으로 primary 로 복구될 수 있으니 조심)
    
3. 교체된 Reader 인스턴스 Scale-up

## 3. AWS Aurora 시점 복원

DB 클러스터 특정 시점으로 복원

1. 복원하려는 DB 클러스터 선택 
2. 작업 > 특정 시점으로 복원
3. 사용자 지정 날짜 및 시간 선택 후 시점 입력

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%206.png)

1. DB 클러스터 정보 입력 후 특정 시점으로 복원

<aside>
💡 여기서 복원의 경우 기존 DB클러스터를 복원하는 것이 아닌 해당 시점의 데이터로 새로운 클러스터를 생성하는 것임

</aside>

[DB cluster를 특정 시간으로 복원](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/USER_PIT.html)

## 4. AWS DMS(Database Migration Service)

AWS Database Migration Service(AWS DMS)는 RDB, DW, NoSQL DB 등 데이터 저장소를 쉽게 마이그레이션 할 수 있는 클라우드 서비스이다. 일회성 마이크레이션을 수행할 수 있으며 지속적인 변경 사항을 복제하여 소스와 대상을 동기화 상태로 유지할 수 있다. 또한 AWS Schema Conversion Tool(AWS SCT)를 사용하여 대상 테이블, 인덱스, 트리거 등 일부 또는 전체 데이터를 생성할 수 있다.

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%207.png)

작업은 세 가지 주요 단계로 구성된다.

1. 기존 데이터의 전체 로드
2. 캐시된 변경 사항 적용
3. 지속적인 복제

테이블 → 테이블 로드

전체 로드가 진행되는 동안 로드 중인 테이블에 대한 변경 사항은 복제 서버에 캐시

AWS DM 는 해당 테이블에 대한 전체 로드가 시작될 때까지 해당 테이블의 변경 사항을 캡쳐하지 않는다. 

주어진 테이블에 대한 전체 로드가 완료되면 AWS DMS는 즉시 해당 테이블에 대해 캐시된 변경 사항을 적용하낟. 테이블이 로드되고 캐시된 변경 사항이 적용되면 진행 중인 복제 단계에 대한 트랜잭션으로 변경 사항을 수집하기 시작. 트랜잭션에 완전히 로드되지 않은 테이블이 있는 경우 변경 사항은 복제 인스턴스 로컬로 저장

### 4.1 AWS DMS의 구성 요소

AWS DMS 마이크레이션은 복제 인스턴스, 소스 및 대상 엔드포인트, 복제 작업 세 가지 구성 요소로 구성된다.

![Untitled](AWS%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20%E1%84%83%E1%85%A2%E1%84%8B%E1%85%B3%E1%86%BC%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%206600520bada04544bac742bce145c723/Untitled%208.png)

[What is AWS Database Migration Service?](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html)

## 5. Python 배치작업

alimtalk/kakaotalk 위주

Python Scripts 생성 > 드웨인 컨펌 > test-dwayne 까지 적용해보기