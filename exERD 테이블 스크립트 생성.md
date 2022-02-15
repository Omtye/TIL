# exERD 테이블 스크립트 생성

상태: 진행 중

## 1. 테이블 표시 정보

테이블 선택 > 컬럼보기 설정 

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled.png)

테이블 스키마 표시 항목 

- 현재 모드 이름 : 물리 이름
- 타입 : 데이터 타입
- 반대 모드 이름 : 논리 이름
- 널 허용 : 널 허용 여부 표시

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled%201.png)

## 2. 스키마 입력 확인

테이블 오른쪽 마우스 > 특성 

스키마 논리, 물리 정상적으로 입력되었는지 확인

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled%202.png)

## 3. 컬럼 설정

### 3.1 PK 설정

지정하려는 컬럼 > PK 컬럼으로 지정

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled%203.png)

### 3.2 Auto Increment

테이블 오른쪽 마우스 > 특성 > 컬럼 > 해당 컬럼 선택 후 자동 증가 체크

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled%204.png)

### 3.3 DateTime

일자 컬럼의 경우 DATETIME 으로 지정 

기본 값 표현식 

- CURRENT_TIMESTAMP : Insert 시 현재 시각이 입력됨 (등록일시)
- CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP : update 시 해당 시점 시간(수정일시)

![Untitled](exERD%20%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20fc5d81fe994146b59b5241f95bd3b3cd/Untitled%205.png)

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