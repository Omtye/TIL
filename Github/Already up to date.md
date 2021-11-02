master 코드 변경 후  pull 요청 시

"Already up to date" 문구가 표시될 경우 아래의 명령어로 원격 저장소를 모두 fetch 한 후 해당 브랜치로 강제 리셋시키면 된다.



```
$git fetch --all
$git reset --hard origin/master
```



**현재 로컬에서 작업한 내역이 사라질 수 있으니 극히 주의!!!!!!!!!!!!!!**

