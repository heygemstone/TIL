--2018-06-20

--왜 관계형DB인가? 평소에는 독립적으로 존재하다가 필요할 때 관계를 맺는다.
--ALIAS : COLUMN ALIAS- SELECT에 씀 // TABLE ALIAS - FROM에 씀 
--Pseudo Column 예 (rownum)
--종류: ROWNUM은 변경된다 
--ROWID: 모든 데이터에 고유하게 붙어있는 ROW의 식별자
--INDEX에 ROWID가 저장이 됨. ROWID를 알면 해당데이터가 어디에 위치하는지 안다
--ROW의 저장위치를 가리키는 주소값을 말한다

--NAVER 고객데이터 2천만건 ID PWD
--SCAN(5가지 방법이 있음) 찾다, 조사하다
--1)FULL TABLE SCAN (처음부터 끝까지 조사)
--2)INDEX SCAN (인덱스 스캔 방법)
--ㅇ-이씨먼저 검색하면 된다(정렬된데이터의 장점) 데이터를 다 스캔할필요없이 필요한부분으로 바로 가면 된다 
--
--SEQUENCE.NEXTVAL / SEQUENCE.CURR
--SEQUENCE:일렬번호 자동생성기 
--인덱스 수업(9월) 경력직 사원들도 인덱스가 무엇인지 구조, 아키텍처를 모르는 경우가 많다
--성능 관점에서 매우 중요한 개념이다
--INDEX: 색인 (전화번호부//사전)
--1)QUICK SEARCH 2) 

select ename, rownum, rowid from emp;

SELECT * ROWNUM, ROWID FROM EMP;

SELECT * FROM CUSTOMER WHERE NAME LIKE '%!_%' ESCAPE '!';
SELECT * FROM CUSTOMER WHERE NAME LIKE '%\_%' ESCAPE '\';
--따로 ESCAPE문자를 지정해주어야한다.(LIKE 뒤에서)
--데이터 암호화/복호화 수업 (9월)
--사소한 기법: VIEW를 쓰면 개발자들은 ID PWD를 볼수 없다 (그전에는 다 볼수 있었다)
--개인의 민감한 데이터는 의무적으로 암호화했다.


--새로운 COLUMN생성

--ESCAPE옵션은 사용자 마음대로 지정할 수 있다.
--오라클 용어로는 와일드카드 문자 

SELECT SYSDATE, TO_CHAR(SYSDATE,'SSSSS') FROM DUAL;
--하루를 초단위로 표현해 자정부터 현재까지 흐른 초를 표시해줌 (하루는 86400초)
--SYSDATE는 년월일까지만표기해줌??
SELECT SYSDATE, TO_CHAR(SYSDATE,'DAY') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'DY') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'D') FROM DUAL; --요일지정숫자
SELECT SYSDATE, TO_CHAR(SYSDATE,'DD') FROM DUAL; --해당 월을 기준으로 일을 RETURN
SELECT SYSDATE, TO_CHAR(SYSDATE,'DDD') FROM DUAL; --1월1일기준 흐른 일을 RETURN
SELECT SYSDATE, TO_CHAR(SYSDATE,'MON') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'MONTH') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'MM') FROM DUAL; --날짜를 문자로 표현한다.
SELECT SYSDATE, TO_CHAR(SYSDATE,'YYYY') FROM DUAL; --현재날짜기준
SELECT SYSDATE, TO_CHAR(SYSDATE,'YY') FROM DUAL;
SELECT TO_CHAR(TO_DATE('82/11/11','RRRR/MM/DD'),'RRRR/MM/DD') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'RRRR') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'RR') FROM DUAL;
SELECT SYSDATE, TO_CHAR(SYSDATE,'Year') FROM DUAL;--대소문자 구분함
SELECT SYSDATE, TO_CHAR(SYSDATE,'YEAR') FROM DUAL;
