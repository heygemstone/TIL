select * from emp;
--각각의 세션은 독립된 세션임 

SELECT ENAME, SYSDATE, HIREDATE FROM EMP;

--이 세션에서만 적용가능
--세션 하나 더 만들면 적용이 안되어있음
--세션의 의미 및 역할
--세션이란 로그인- 로그아웃까지 클라이언트의 상태를 관리하기 위한 상태정보를 말한다.
--STATUS INFORMATION
--DBMS와 세션정보를 모니터링하는 것도 SQL로 가능하다. 

--SID: 세션 아이디

SELECT * FROM V$MYSTAT;

--나는 데이터를 들여다 볼수없다. 관리자만 볼수있음
--V:VIRTUAL의 약자 STATISTICS: 통계의 약자
--MYSTAT: 내 통계 (세션정보가 들어가있음)

--SQL 코딩지침 (reserved word 대문자 소문자 한칸띄우기 두칸띄우기 등등)
--SQL Script 개발지침 
--왜 실무에서 SQL SCRIPT를 쓸까??
--1)재사용
--2)N개의 SQL을 연속해서 실행할 수 있다.
--3)리눅스의 쉘스크립트와 SQL스크립트 의미 목적이 같음
@C\99_SQL\VICTIM.SQL
--@경로+파일명을 넣으면 SQL스크립트를 실행할 수 있다. 

--구글 뒤지면 sql가이드라인을 찾아볼수있다. sql문장에 주석다는 이유?
--소스코드품질을 일정수준으로 끌어올리려고 하는 것 

SELECT CEIL(DBMS_RANDOM.VALUE(0.1,22)) AS VICTIM FROM DUAL;

------------------------
--단일행 - 날짜함수
------------------------
SELECT HIREDATE, MONTHS_BETWEEN(SYSDATE, HIREDATE), MONTHS_BETWEEN(HIREDATE, SYSDATE)
FROM EMP;

--날짜에서 TRUNC/ ROUND 시간(오전오후) 1분기 2분기 끊을 수 있다
SELECT SYSDATE, ADD_MONTHS(SYSDATE,3), ADD_MONTHS(SYSDATE,-1) FROM DUAL;

SELECT SYSDATE, LAST_DAY(SYSDATE), NEXT_DAY(SYSDATE,'일요일'),
    NEXT_DAY(SYSDATE,1), NEXT_DAY(SYSDATE,2)
    FROM DUAL;
    --지정한날짜의 다음 요일 날짜를 반환. 요일마다 할당된 숫자가 있음 

SELECT SYSDATE, ROUND(SYSDATE,'YEAR'), ROUND(SYSDATE,'MONTH'),
                ROUND(SYSDATE+2,'DAY'), ROUND(SYSDATE)
                FROM DUAL;
                
                
                
SELECT SYSDATE, TRUNC(SYSDATE,'YEAR'), ROUND(SYSDATE,'MONTH'),
                TRUNC(SYSDATE,'DAY'), TRUNC(SYSDATE)
                FROM DUAL;

SELECT TO_CHAR(SYSDATE,'MM"월"DD"일"')AS MMDD1,
            TO_CHAR(SYSDATE,'MM')||'월'||TO_CHAR(SYSDATE,'DD')||'일' AS MMDD2
            FROM DUAL;
            
SELECT EXTRACT (YEAR FROM SYSDATE),
       EXTRACT (MONTH FROM SYSDATE),
       EXTRACT (DAY FROM SYSDATE)
FROM DUAL;

SELECT HIREDATE, EXTRACT(YEAR FROM HIREDATE) FROM EMP;


--날짜쓰는 포맷안에 문자를 쓸수있다. 여기는 ""더블쿼테이션을 쓴다.

--(확장)해당월의 마지막  법정 영업일자를 구하십시요
--(법정영업일은 월~금요일,법정 공휴일)  Stored Function 이용 
--LAST_DAT로구하기 그런데 토요일이나 일요일이면 금요일로 당겨야한다

SELECT TO_CHAR(LAST_DAY(SYSDATE),'D') FROM DUAL;

SELECT TO_CHAR(SYSDATE,'DDD'),TO_CHAR(SYSDATE,'DD'),TO_CHAR(SYSDATE,'D') 
FROM DUAL;
SELECT TO_CHAR(SYSDATE,'DAY'),TO_CHAR(SYSDATE,'DD'),TO_CHAR(SYSDATE,'D') 
FROM DUAL;

SELECT LAST_DAY(SYSDATE) FROM DUAL;
--버전1
SELECT CASE TO_CHAR(LAST_DAY(SYSDATE),'dy')
        WHEN '토' THEN LAST_DAY(SYSDATE)-1
        WHEN '일' THEN LAST_DAY(SYSDATE)-2
        ELSE LAST_DAY(SYSDATE)
        END
        FROM DUAL;
--버전2
SELECT CASE to_char(LAST_DAY(SYSDATE),'d')
        WHEN '7' THEN LAST_DAY(SYSDATE)-1
        WHEN '1' THEN LAST_DAY(SYSDATE)-2
        ELSE LAST_DAY(SYSDATE)
        END
        FROM DUAL;
--버전3        
SELECT CASE to_char(LAST_DAY(SYSDATE),'DAY')
        WHEN '토요일' THEN LAST_DAY(SYSDATE)-1
        WHEN '일요일' THEN LAST_DAY(SYSDATE)-2
        ELSE LAST_DAY(SYSDATE)
        END
        FROM DUAL;
        
SELECT TO_DATE('81/02/20', 'YYYY-MM-DD') AS YYYY, 
       TO_CHAR(TO_DATE('81/02/20', 'YYYY-MM-DD'), 'YYYY-MM-DD') 
       FROM DUAL;

SELECT TO_DATE('81/02/20', 'RRRR-MM-DD') AS YYYY, 
       TO_CHAR(TO_DATE('81/02/20', 'RRRR-MM-DD'), 'YYYY-MM-DD') 
       FROM DUAL;

SELECT to_date('82/02/02','yyyy/mm/dd') as yyyydate,
       to_char(to_date('81/02/20','yyyy/mm/dd'), 'yyyy-mm-dd') as yyyychar
       from dual;
       

SELECT to_date('82/02/02','rrrr/mm/dd') as rrrrdate,
       to_char(to_date('81/02/20','rrrr/mm/dd'), 'rrrr-mm-dd') as rrrrchar
       from dual;       
       --to_date를 먼저 해주고 문자열로 바꿔야함! 
        
SELECT TO_CHAR(to_date('81/02/20','yyyy/mm/dd')), 
       TO_CHAR(to_date('81/02/20','rrrr/mm/dd')) 
       FROM DUAL;
--RR포맷: 년도를 두자리만 출력하는 경우 00-49는 앞에 20을 붙이고,
--50-99는 19를 붙여서 1950-1999의 값을 가지도록 함

SELECT EXTRACT(YEAR FROM SYSDATE) FROM DUAL;
--숫자는 오른쪽 정렬
SELECT EXTRACT(YEAR FROM SYSDATE),
       EXTRACT(MONTH FROM SYSDATE),
       EXTRACT(DAY FROM SYSDATE)
       FROM DUAL;

--가상의 컬럼
--PSEUDO COLUMN:
--ROWNUM/ ROMID:
--ROWNUM은 쿼리에서 반환하는 각 ROW(행)에 순서값을 매기는 가상 COLUMN이다.
--ROWNUM이 많이쓰이고, 주로 조회하는 데이터의 행 번호를 매기는데 쓴다.
SELECT * FROM (SELECT*FROM EMP ORDER BY SAL ASC) WHERE ROWNUM <= 5;


