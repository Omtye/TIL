### [AWS] EB NotAuthorizedError - Operation Denied. Signature expired 오류





eb deploy 로 배포를 진행하려 하니 인증 실패와 서명이 만료되었다는 오류가 발생했다. 



IAM을 확인해보았으나 문제가 없었는데 로컬 PC의 시간이 변경되어 실제 시간과 차이가 발생하여 접속이 불가하였다. 로컬 시간을 동기화 한 후 재실행 하니 정상적으로 동작한다.