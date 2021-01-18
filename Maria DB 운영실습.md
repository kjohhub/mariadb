# MariaDB 쿼리



## 1. 데이터베이스 생성

~~~mariadb
CREATE DATABASE shopdb;
~~~



## 2. 테이블 생성

```mariadb
CREATE TABLE memberTbl(
	memberId CHAR(8) NOT NULL PRIMARY KEY,
	memberName CHAR(5) NOT NULL,
	memberAddress CHAR(20)
);
```



## 3. 데이터 입력

```mariadb
INSERT INTO membertbl (memberId, memberName, memberAddress) VALUES
	('Dang', '당탕이', '경기 부천시 중동'),
	('Jee', '지운이', '서울 은평구 증산동'),
	('Han', '한주연', '인천 남구 주안동'),
	('Sang', '상길이', '경기 성남시 분당동'); 
```



## 4. 데이터 활용

```mariadb
SELECT * FROM membertbl;
```

```mariadb
SELECT * FROM membertbl WHERE memberName = '지운이';
```



## 5. 인덱스

>책의 뒤에 붙어있는 색인과 같은 개념
>
>실무에서 사용되는 대용량 데이터의 경우 필요
>
>데이터베이스 기능을 향상시키는 튜닝에서 중요한 요소 - 쿼리문에 대한 응답 속도 향상
>
>데이터의 열 단위에 생성


```mariadb
CREATE INDEX idx_indextbl_firstname ON indextbl(first_name);
```
> 인덱스 생성후의 실행 계획 확인

```mariadb
EXPLAIN SELECT * FROM indextbl WHERE first_name  = 'Mary';
```



## 6. 뷰

> 가상의 테이블 - 실제 행 데이터를 가지고 있지 않음
>
> 사용자의 입장에서는 테이블과 동일하게 보임
>
> 실체는 없는 것이며, 진짜 테이블에 링크된 개념
>
> 개인 정보 보호와 같은 데이터 보호/보안을 위해서도 사용

```mariadb
CREATE VIEW uv_membertbl
AS
	SELECT membername, memberaddress FROM membertbl;

```

```mariadb
SELECT * FROM uv_membertbl;
```



## 7. 스토어드 프로시저

> MariaDB에서 제공해주는 프로그래밍 기능
>
> SQL 문을 하나로 묶어서 편리하게 사용하는 기능
>
> 실무에서는 SQL 문을 하나씩 실행하는 것보다 스토어드 프로시저로 관리하는 경우가 많음

```mariadb
DELIMITER //
CREATE PROCEDURE myProc()
BEGIN
	SELECT * FROM membertbl WHERE membername = '당탕이';
	SELECT * FROM producttbl WHERE productname = '냉장고';
END //
DELIMITER;
```

```mariadb
CALL myProc() 
```



## 8. 트리거

> 테이블에 부착되어서, 테이블에 INSERT나 UPDATE 또는 DELETE 작업이 발생되면 실행되는 코드

```mariadb
DELIMITER //
CREATE TRIGGER trg_deletedMemberTbl
	AFTER DELETE
	ON memberTbl
	FOR EACH ROW
BEGIN
	INSERT INTO deletedmembertbl
		VALUES (OLD.memberId, OLD.memberName, OLD.memberAddress, CURDATE());
END //
DELIMITER;
```

```mariadb
DELETE FROM membertbl WHERE memberName = '지운이';
```

