## Aurora RDS 무중단 Scale-Up

Aurora에서 무중단으로 Scale-up 할 경우가 필요할 수 있다. 이러한 경우 읽기 인스턴스에 대해 Scale-up을 진행합니다. 이후 Failover를 통해 라이터 인스턴스로 승격시킨다. (기존의 라이터 인스턴스는 리더로 변경됨). **이 때 tier 우선 순위에 따라 기존 인스턴스가 자동으로 primary 로 복구될 수 있으니 조심해야한다.**



**순서** 

1. Scale-up 대상 RDS의 Reader 인스턴스 Scale-up
2. Failover를 통해 Writer와 Reader를 교체한다. 
   
    (이 때 tier 우선 순위에 따라 기존 인스턴스가 자동으로 primary 로 복구될 수 있으니 조심)
    
3. 교체된 Reader 인스턴스 Scale-up

