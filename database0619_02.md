-------------
--rownum: row에 부여된 번호를 뜻한다.
-------------
--rownum이란 column은 emp테이블에 존재하지 않는다.

--Pseudo Column: 존재하지 않는 Column 정의/설명 숙제
--쿼리 내에서 사용 가능한(실제 컬럼이 아닌) 가상의 열.(조회가능)
--ROWNUM이 ORDER BY 보다 먼저 실행됨
--ROWNUM은 절대번호가 아님 

SELECT ROWNUM, ENAME, DEPTNO, SAL FROM EMP;
--1)FROM 2)WHERE 3)ROWNUM(WHERE 수행한 결과에 대해 부여하는 번호!) 4)ORDER BY
SELECT ROWNUM, ENAME, DEPTNO, SAL FROM EMP ORDER BY DEPTNO, SAL;
--여기선 번호 붙이고 ORDER BY해서 SCOTT이 8번
--실행순서 체크하기 WHERE (결과집합생성) ROWNUM 부여 ORDER BY
--ROWNUM 결과집합 RESULT SET에 부여된 번호 
SELECT ROWNUM, ENAME, DEPTNO, SAL FROM EMP WHERE DEPTNO IN(10,20) ORDER BY DEPTNO, SAL;
--여기선 WHERE절 정렬하고 번호를 붙여서 SCOTT이 4번
--조건절에서 ROWNUM사용시 주의사항
SELECT ENAME, DEPTNO, SAL FROM EMP WHERE ROWNUM =1;

SELECT ENAME, DEPTNO, SAL FROM EMP WHERE ROWNUM =5;--안되는이유
SELECT ENAME, DEPTNO, SAL FROM EMP WHERE ROWNUM >5;
--WHERE절이 먼저 실행 WHERE 뒤에 추가 조건이 없기 때문에 유추가 안된다. 그래서 뜨지 않음

SELECT ENAME, DEPTNO, SAL FROM EMP WHERE ROWNUM<=5;
SELECT ENAME, DEPTNO, SAL FROM EMP WHERE ROWNUM<5;

--최상위 급여자 5명을 조회하는 SQL문을 작성하라
SELECT ENAME, SAL 
        FROM (
        SELECT ENAME, SAL
        FROM EMP 
        ORDER BY SAL DESC)
        WHERE ROWNUM <=5;
        
-------------        
--논리연산자
-------------
SELECT ENAME, JOB, SAL, DEPTNO FROM EMP WHERE DEPTNO =10 AND SAL >2000;
SELECT ENAME, JOB, SAL, DEPTNO FROM EMP WHERE DEPTNO =10 OR SAL > 2000;
SELECT ENAME, JOB, SAL, DEPTNO FROM EMP WHERE SAL > 2000 OR SAL > 2000;
SELECT ENAME, JOB, SAL, DEPTNO FROM EMP WHERE SAL > 1000 OR SAL > 2000;
--Optimizer 
--연산자 우선순위, 좋은 방식의 SQL은? Optimizer는 누구를 좋아하는가?
--WHERE (A and B) OR C:  AND가 먼저다!
--WHERE A and (B OR C): 괄호쳐주는 습관이 필요하다. 최우선은 괄호! 

SELECT ENAME, JOB, SAL, DEPTNO FROM EMP
WHERE (SAL > 2000 AND DEPTNO = 20) OR JOB = 'CLERK';
--
SELECT ENAME, JOB, SAL, DEPTNO FROM EMP
WHERE SAL > 2000 AND (DEPTNO =20 OR JOB = 'CLERK');
--AND먼저 실행된다. 

SELECT ENAME, JOB, SAL, DEPTNO FROM EMP
WHERE (DEPTNO =10 AND SAL > 2000) OR JOB = 'CLERK';

SELECT ENAME, JOB, SAL, DEPTNO FROM EMP
WHERE DEPTNO = 10 AND (SAL > 2000 OR JOB = 'CLERK');
--AND와 OR중에 AND가 먼저 실행됨. AND > OR
--SQL Optimezer(최적화기)기술투자를 많이하고 있음
--방법과 순서를 고민함 (책사 역할) 
--전체 일의 양을 줄일 수 있기 때문에 AND를 먼저 처리함
SELECT ENAME, JOB, SAL FROM EMP WHERE JOB != 'CLERK';
SELECT ENAME, JOB, SAL FROM EMP WHERE JOB NOT IN('CLERK', 'MANAGER');

SELECT ENAME, JOB, SAL FROM EMP WHERE SAL > 2000 OR JOB = 'CLERK';
SELECT ENAME, JOB, SAL FROM EMP WHERE SAL > 2000 AND JOB = 'CLERK';

--OR연산시 중복되는 ROW는 어떻게 처리되는지 아래 SQL실행후 결과를 설명하시오
--DEPTNO=20  OR JOB = 'CLERK'를 둘다 만족하는 ROW는 2번 출력되는가?
SELECT DEPTNO, JOB, ENAME FROM EMP WHERE DEPTNO=20;
SELECT DEPTNO, JOB, ENAME FROM EMP WHERE JOB = 'CLERK';
SELECT DEPTNO, JOB, ENAME FROM EMP WHERE DEPTNO =20 OR JOB = 'CLERK';
--중복처리됨
SELECT DEPTNO, JOB, ENAME FROM EMP WHERE DEPTNO = 20 
INTERSECT SELECT DEPTNO, JOB, ENAME FROM EMP WHERE JOB = 'CLERK';
--중복항목 조회
SELECT DEPTNO, JOB, ENAME FROM EMP WHERE DEPTNO = 20
UNION ALL SELECT DEPTNO, JOB, ENAME FROM EMP WHERE JOB = 'CLERK';
--OR는 사실 UNION
--BETWEEN
--SQL연산자 (LIKE BETWEEN LIST IN)
SELECT EMPNO, ENAME, SAL FROM EMP WHERE SAL BETWEEN 1000 AND 2000; 
--아래와 같은결과
SELECT EMPNO, ENAME, SAL FROM EMP WHERE SAL >= 1000 AND SAL <= 2000;

SELECT EMPNO, ENAME, HIREDATE, SAL FROM EMP WHERE SAL BETWEEN 2000 AND 1000;
--??큰거부터 적으면 안된다. BETWEEN A AND B에서 A: LOWER BOUND B: UPPER BOUND
--BETWEEN연산자는 자동으로 <= >=으로 바꿔준다 (약간 사기)
SELECT EMPNO, ENAME, HIREDATE, SAL FROM EMP WHERE ENAME BETWEEN 'C' AND 'K';
--문자도 가능, DATA TYPE에 관계없이 BETWEEN을 쓸수있다. 맨앞글자부터 비교함
SELECT EMPNO, HIREDATE, SAL FROM EMP 
WHERE HIREDATE 
BETWEEN '81/02/20' AND '82/12/09';
--DATA TYPE CONVERSION 발생 좋은 방법아님

SELECT ENAME, HIREDATE, SAL FROM EMP 
WHERE HIREDATE BETWEEN TO_DATE('81/02/20','yy/mm/dd')
AND TO_DATE('82/12/09','yy/mm/dd');--출력안됨
--날짜 표시 형식이 다름 YY로하면 2081년으로 인식함 
SELECT ENAME, HIREDATE, SAL FROM EMP
WHERE HIREDATE BETWEEN TO_DATE('81/02/20', 'rr/mm/dd')
AND TO_DATE('82/12/09','rr/mm/dd');--출력됨
--날짜포맷 YY와 RR의 차이점?
--YY형식으로하면 1981년이 아니라 2081년으로 인식함
--요구
SELECT * FROM SALGRADE WHERE 3000 BETWEEN LOSAL AND HISAL;

-------------
--IN 정의: 리스트 연산자
-------------
SELECT EMPNO, ENAME, JOB FROM  EMP WHERE EMPNO IN(7369,7521,7654);
--OR로 해석됨(이거나 이거나 이거나)간결한 표현이 가능하기 때문에 사용한다.
SELECT EMPNO, ENAME, JOB FROM EMP WHERE EMPNO = 7369 OR
EMPNO =7521 OR EMPNO = 7654;

SELECT EMPNO, ENAME, JOB FROM EMP WHERE JOB IN ('CLERK','MANAGER');
//DATA는 대소문자를 구분한다.

SELECT EMPNO, ENAME, HIREDATE FROM EMP WHERE HIREDATE IN('81/05/01', '81/02/20');
--암시적 DATA TYPE CONVERSION이 발생함 
--DATE TYPE에서 DEFAULT값이 무엇인가? RR인 것 같다.
--BETWEEN연산시 YY는 안나오는데 RR은 나옴
--1990년데 디폴트값은??
--OR로 기계적으로 바꿈 오라클내부에서

SELECT EMPNO, ENAME, JOB, DEPTNO FROM EMP
WHERE (JOB, DEPTNO) IN (('MANAGER',20),('CLERK',20));
--안에꺼는 AND로 보고 OR로 출력함 MANAGER이고 20번이어야 함 동시비교
--JOB과 DEPTNO은 AND 관계이다. 
SELECT EMPNO, ENAME, JOB FROM EMP WHERE EMPNO IN (7369, 7369, 7654);
--중복항목 한번만 나옴 OR

SELECT * FROM DEPT;
--부서정보를 저장하는 테이블
SELECT * FROM EMP;
--사원정보를 저장하는 테이블

SELECT DEPTNO, ENAME, JOB FROM C##03.EMP
WHERE DEPTNO IN 
(SELECT DEPTNO FROM C##03.DEPT 
               WHERE LOC IN ('NEW YORK','CHICAGO'));
--부서이름만 알고 거기서 근무하는 사람을 알고 싶을 떄 
--부서이름(N,C)(간접적 정보)만 갖고서 사원정보를 검색할 수 있다
--굉장히 유연하게 사용할 수 있다. 서브쿼리의 장점. 
--부서가 뉴욕이거나 시카고인 사원 조사 OR관계임.
--1)서브쿼리 정의 및 설명
--SQL문 안에 들어가있는 SELECT 문장이 SUBQUERY
--바깥 SQL문은 MAIN QUERY:SELECT  
--2)SCOTT.EMP 설명 :SCOTT 소유의 EMP 테이블 (자신의 계정으로 바꿔야 한다) 
--3)정상적 결과가 나오도록 SQL 수정하기 : 띄어쓰기 수정해야 함

-------------
--ANY(=SOME), ALL ANY와 SOME은 같은 의미
-------------
--비교연산자는 단일값 비교이다. 그래서 여러개의 값을 주면 에러가 난다.
SELECT ENAME, SAL FROM EMP WHERE SAL > (1300, 2450, 3000);--에러남
SELECT ENAME, SAL FROM EMP WHERE SAL > ANY (1300, 2450, 3000); 
--OR적용 1300보다 큰 값이 나옴 
SELECT ENAME, SAL FROM EMP WHERE SAL > ALL (1300, 2450, 3000);

SELECT ENAME, SAL FROM EMP WHERE SAL = ANY (1300,2450,3000);
--OR의개념이라 세 값이 모두 적용됨
SELECT ENAME, SAL FROM EMP WHERE SAL = SOME (1300, 2450, 3000);
--ANY와 SOME은 같은 개념
SELECT ENAME, SAL FROM EMP WHERE SAL = ALL (1300, 2450, 3000);
--ALL AND의 의미 세가지 값을 모두 만족하는 경우는 없다.

--LIKE 정의: 문자 패턴 매칭 연산자, 정확한 값을 몰라도 찾을 수 있다.
-- %: 0개이상의 모든문자
-- _: 1개의 모든문자, 위치가 의미를 가짐

SELECT ENAME FROM EMP WHERE ENAME = '%\_A';
SELECT * FROM TAB;
--CUSTOMER(SYNONYM)빼고 다른 테이블 우리가 수정할 수 있는 권한이 있음

SELECT ENAME FROM EMP UPDATE WHERE ENAME = 'A_AA';

-------------
--FUNCTION
-------------
--1)ORACLE 제공 함수
--2)사용자 정의 함수 (PL/SQL)로 사용자가 작성할 수 있음
--편리하게 사용하기 위해 
--function 단일행함수 /그룹행함수
--1)단일행함수 single row function (substring/length)\
--문자/숫자/날짜/형변환/기타함수
--2)그룹행함수 group row function (count/sum/avg...)
--함수는 입력과 출력을 가진다. 함수는 단 하나의 값만 return함
--출력은 단일행! 입력은 단일행/그룹행
--입력의 갯수로 단일행, 그룹행 함수를 나눈다. 입력이 단일 값? 그룹 값이냐에 따라 나눔
-----single row function
SELECT ENAME, EMPNO, SAL, COMM FROM EMP;
SELECT ENAME, LOWER(ENAME), UPPER(LOWER(ENAME)), 
        LENGTH(ENAME), ABS(SAL-EMPNO), COMM
        FROM EMP;
        
SELECT ENAME, SUBSTR(ENAME,1,3) FROM EMP
        WHERE HIREDATE between to_date('81/01/01','RR/MM/DD') 
        and to_date('82/12/31','rr/mm/dd')
        ORDER BY by length(ENAME); --고치기
        
--그룹행 함수

SELECT AVG(SAL), SUM(SAL), SUM(COMM), COUNT(*) FROM EMP;
SELECT DEPTNO, COUNT(*), SUM(SAL), AVG(SAL) FROM EMP 
        GROUP BY DEPTNO;
        
SELECT DEPTNO, JOB, COUNT(*), SUM(SAL), AVG(SAL) 
FROM EMP
GROUP BY DEPTNO, JOB; //그룹행함수에 대해 GROUP BY 가능함

--PAD: 채우다
--PAD (IT용어) RPAD(오른쪽 채우기) LPAD(왼쪽 채우기)
--TRIM (공백 잘라내기) (오른쪽 자르기 왼쪽 자르기)
--CONCAT: 문자열 합성
--REPLACE: 치환하기 
--ASCII: 코드값 반환하기
--CHR: ASCII 코드를 넣으면 문자로 변환하기
--공백문자도 엄연한 데이터이다

-----
--단일행 문자 함수
-----
SELECT ENAME, LOWER(ENAME), UPPER(ENAME), INITCAP(ENAME) FROM EMP;
SELECT ENAME, substr(ENAME,1,3), substr(ENAME,4), substr(ENAME,-3,2) FROM EMP;
SELECT ENAME, instr(ENAME,'A'), instr(ENAME,'A',2), instr(ENAME,'A',1,2) FROM EMP;
--INSTR 문자열내 특장문자의 위치값을 반환하는 함수
SELECT * FROM EMP;
SELECT ENAME, rpad(ENAME,10,'*'), lpad(ENAME,10,'+') FROM EMP;

SELECT rpad(ENAME,10,' ')||' ''s JOB is '||lpad(JOB,10,' ') as JOB_list FROM EMP;

--공백문자 인식

SELECT ENAME, REPLACE(ENAME,'S','s') FROM EMP;
SELECT ENAME, concat(ENAME, JOB), ENAME||JOB FROM EMP;

SELECT ltrim(' 대한민국 '), rtrim(' 대한민국 '), trim(' 대한민국 ') FROM DUAL;
SELECT TRIM('장' FROM '장발장'), LTRIM('장발장','장'), RTRIM('장발장','장')FROM DUAL;

SELECT length('abcd'), substr('abcd',2,2), length('대한민국'),
        substr('대한민국',2,2) FROM DUAL;
SELECT lengthb('abcd'), substr('abcd',2,2), lengthb('대한민국'),
        substrb('대한민국',2,2) FROM DUAL;
        
SELECT length('abcd'), vsize('abcd'), length('대한민국'), vsize('대한민국') from dual;

--단일행 숫자함수

SELECT round(45.923,2), round(45.923,1), round(45.923,0),
        round(45.923), round(45.923,-1) from DUAL;
---1의의미는?? 첫번째 자리를 반올림해버린다.
SELECT TRUNC(45.923,2), TRUNC(45.923,1), TRUNC(45.923,0),
        TRUNC(45.923), TRUNC(45.923,-1) FROM DUAL;
---1은 첫번째 자리를 버린다??

SELECT MOD(100,3), MOD(100,2) FROM DUAL; --나머지
SELECT ENAME, SAL, SAL*0.052 AS TAX, 
        ROUND(SAL*0.053,0) AS R_TAX 
        FROM EMP; -- 급여의 5.3% 세금, 원단위로(반올림해주기) 
        --세금은 손해보면 안되니까

SELECT CEIL(-45.594), CEIL(-45.294), CEIL(45.294),
        ROUND(-45.494), ROUND(-45.294), ROUND(45.594) 
        FROM DUAL;
--CEIL입력된 값보다 큰 정수중에 가장 작은 값을 반환. 
--ROUND 반올림(음수 반올림) --절대값연산
SELECT FLOOR(45.245), FLOOR(-45.245), FLOOR(45.545), FLOOR(-45.545)
        FROM DUAL;
--FLOOR 입렵된 값보다 작은 정수중 가장 큰 값 반환.


SELECT sysdate, sysdate +7, sysdate -2, 
        sysdate +1/24 from dual; --한시간뒤라는 의미 sysdate +1/24
SELECT DEPTNO, ENAME, TRUNC(SYSDATE - HIREDATE) AS WORK_DAY
        FROM EMP ORDER BY DEPTNO, WORK_DAY DESC;
        
--왜 날짜만 보이고 시간은 보이지 않는가? 

SELECT VSIZE(HIREDATE) FROM EMP;
--DATE: 7BYTE // 날짜 & 시간 EX)YYYYMMDDHH24MISS
SELECT ENAME, TO_CHAR(SYSDATE,'YYYY-MM-DD:HH24:MI:SS'),
        TO_CHAR(HIREDATE,'YYYY-MM-DD:HH24:MI:SS')
        FROM EMP;

ALTER SESSION SET NLS_DATE_FORMAT ='YYYY-MM-DD:HH24:MI:SS';

--ALTER SESSION SET 세션정보 변경 날짜표시포맷을 시분초까지 표기하라
--NLS_DATE_FORMAT: 환경변수 (오라클레벨)환경변수가 미치는 범위 
--내 세션 하나만 바뀐다.(내 계정까지만)
--ALTER SYSTEM SET :세션 전부 시스템번부의 날짜포맷을 변경가능함
SELECT ENAME, SYSDATE, HIREDATE FROM EMP;

select sysdate from dual;
