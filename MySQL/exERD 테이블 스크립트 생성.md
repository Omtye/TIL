# exERD 테이블 스크립트 생성

<br>

## 1. 테이블 표시 정보

테이블 선택 > 컬럼보기 설정 

![Untitled 1](https://user-images.githubusercontent.com/43038052/153976654-136c17c5-4c61-4112-98d0-b82420223ec9.png)

<br>

테이블 스키마 표시 항목 

- 현재 모드 이름 : 물리 이름
- 타입 : 데이터 타입
- 반대 모드 이름 : 논리 이름
- 널 허용 : 널 허용 여부 표시

![Untitled 2](https://user-images.githubusercontent.com/43038052/153976674-9a84986a-4938-4f46-87e7-9d1edd4eba45.png)

<br>


## 2. 스키마 입력 확인

테이블 오른쪽 마우스 > 특성 

스키마 논리, 물리 정상적으로 입력되었는지 확인

![Untitled 3](https://user-images.githubusercontent.com/43038052/153976675-acc44079-4e1b-44eb-847c-211c335f1206.png)

<br>

## 3. 컬럼 설정

### 3.1 PK 설정

지정하려는 컬럼 > PK 컬럼으로 지정
![Untitled 4](https://user-images.githubusercontent.com/43038052/153976677-0a12f7a3-02f0-433a-baca-185e3e942cd7.png)

<br>

### 3.2 Auto Increment

테이블 오른쪽 마우스 > 특성 > 컬럼 > 해당 컬럼 선택 후 자동 증가 체크

![Untitled 5](https://user-images.githubusercontent.com/43038052/153976680-949ba9db-0cf9-402f-95b9-5269e7254462.png)

<br>

### 3.3 DateTime

일자 컬럼의 경우 DATETIME 으로 지정 

기본 값 표현식 

- CURRENT_TIMESTAMP : Insert 시 현재 시각이 입력됨 (등록일시)
- CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP : update 시 해당 시점 시간(수정일시)

![Untitled](https://user-images.githubusercontent.com/43038052/153976683-54f95e8b-b062-4c9e-9cab-ea8042e2e9b1.png)

<br>

## 4. 포워딩

메뉴 exERD > 포워드 엔지니어링 > DDL 까지만 진행한 후 복사하여 사용

```sql

CREATE TABLE `abbott`.`tb_test` (
	`id`              BIGINT      NOT NULL COMMENT 'ID', -- ID
	`plus_friend_key` VARCHAR(50) NOT NULL COMMENT '플러스 친구키', -- 플러스 친구키
	`cell`            VARCHAR(50) NOT NULL COMMENT '휴대폰 번호', -- 휴대폰 번호
	`app_user_id`     VARCHAR(50) NOT NULL COMMENT '앱 유저 아이디', -- 앱 유저 아이디
	`reg_dt`          DATETIME    NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '등록 일시', -- 등록 일시
	`upd_dt`          DATETIME    NULL     DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP  COMMENT '수정 일시' -- 수정 일시
)
COMMENT '애보트 회원 인증2';

ALTER TABLE `abbott`.`tb_test`
	ADD CONSTRAINT `PK_tb_test` -- 애보트 회원 인증2 기본키
		PRIMARY KEY (
			`id`,              -- ID
			`plus_friend_key`  -- 플러스 친구키
		);
```

<br>

