# SQL 기본



## 1. SELECT 문

### 1.1 SELECT ... FROM

* 기본 형식

  SELECT * FROM first_name, gender FROM employees;

  ```
  SELECT * FROM employees.titles;
  SELECT * FROM titles;
  ```

  

### 1.2 SELECT ... FROM ... WHERE

* 기본 형식

  ```mariadb
  SELECT * FROM userTbl WHERE name = '김경호'
  ```

* **AND / OR**

  ```mariadb
  SELECT * FROM userTbl WHERE birthYear >= 1970 AND height >= 182;
  ```

  ```mariadb
  SELECT * FROM userTbl WHERE birthYear >= 1970 OR height >= 182;
  ```

* **BETWEEN ... AND**

  ```mariadb
  SELECT * FROM userTbl WHERE height BETWEEN 180 AND 183;
  ```



* **IN**

  ```
  SELECT * FROM userTbl WHERE addr IN ('경남', '전남', '경북');
  ```

* **LIKE**

  ```mariadb
  SELECT * FROM userTbl WHERE name LIKE '김%'
  SELECT * FROM userTbl WHERE name LIKE '_종신'
  ```



* **SubQuery**

  ```mariadb
  SELECT * FROM userTbl
  	WHERE height > (SELECT height FROM userTbl WHERE name = '김경호');
  ```

* **ANY / ALL**

  ```mariadb
  SELECT * FROM userTbl
  	WHERE height >= ANY (SELECT height FROM userTbl WHERE addr = '경남');
  ```

  ```mariadb
  SELECT * FROM userTbl
  	WHERE height >= ALL (SELECT height FROM userTbl WHERE addr = '경남');
  ```



* **ORDER BY**

  ```mariadb
  SELECT * FROM userTbl ORDER BY height DESC, name ASC;
  ```



* **DISTINCT** - 중복제거

  ```mariadb
  SELECT DISTINCT addr FROM userTbl;
  ```



* **LIMIT** - 출력 개수 제한

  ```mariadb
  SELECT * FROM employees LIMIT 5;
  SELECT * FROM employees LIMIT 0, 5;		-- LIMIT 5 OFFSET 0과 동일
  ```




### 1.3 GROUP BY 및 HAVING 그리고 집계 함수

* GROUP BY

  ```mariadb
  SELECT userID, SUM(amount) FROM buyTbl GROUP BY userID;
  ```

  ```mariadb
  SELECT userID AS '사용자 아이디', SUM(amount) AS '총 구매 개수' FROM buyTbl GROUP BY userID;
  ```


* 집계 함수

  > AVG(), MIN(), MAX(), COUNT(), COUNT(DISTICT), STDEV(), VAR_SAMP()

  ```mariadb
  SELECT AVG(amount) AS '평균 구매 개수' FROM buyTbl
  ```

* HAVING

  > 집계함수에서 조건을 제한한다
  >
  > 반드시 GROUP BY 다음에 와야 한다.

  ```mariadb
  SELECT userID AS '사용자', SUM(price*amount) AS '총 구매액'
  	FROM buyTbl
  	GROUP BY userID
  	HAVING SUM(PRICE*amount) > 1000;
  ```

* ROLLUP

  > 총합 또는 중간 합계 출력
  >
  > GROUP BY 절과 함께 사용

  ```mariadb
  SELECT groupName, SUM (price*amount) AS '비용'
  	FROM buyTbl
  	GROUP BY groupName
  	WITH ROLLUP;
  ```





## 2. 데이터의 변경을 위한 SQL문

### 2.1 데이터의 삽입 : INSERT

* 기본 형식

  ```mariadb
  INSERT INTO testTbl(id, userName) VALUES (2, '설현')
  ```

* AUTO_INCREMENT

  ```mariadb
  CREATE TABLE testTbl2
  	(id int AUTO_INCREMENT PRIMARY KEY,
  	userName char(3),
  	age int);
  ```

  ```mariadb
  ALTER TABLE testTbl2 AUTO_INCREMENT=100;	-- 100부터 시작
  SET @@AUTO_INCREMENT_INCREMENT=3			-- 3씩 증가
  ```

* 대량의 샘플 데이터 생성

  ```mariadb
  CREATE TABLE testTbl4 (id INT, fname VARCHAR(50), lname VARCHAR(50));
  INSERT INTO testTbl4
  	SELECT emp_no, first_name, last_name
  	FROM employees.employees;
  ```

  ```mariadb
  CREATE TABLE testTbl5
  	(SELECT emp_no, first_name, last_name FROM employees.employees);
  ```




### 2.2 데이터의 수정 : UPDATE

* 기본 형식

  ```mariadb
  UPDATE testTbl4 SET lname = '없음' WHERE fname = 'Kyoichi';
  ```



### 2.3 데이터의 삭제 : DELETE FROM

* 기본 형식

  ```
  DELETE FROM testTbl4 WHERE fname = 'Aamer';
  ```

* 대용량 데이터 삭제

  ```mariadb
  DELETE FROM bigTbl1;					-- 트랜젝션 로그 기록, 느림
  ```

  ```mariadb
  DROP TABLE bigTbl2;						-- 테이블 자체 삭제
  ```

  ```mariadb
  TRUNCATE TABLE bigTbl3;					-- 트랜젝션 로그 기록 안함, 테이블 구조는 남김
  ```

  

