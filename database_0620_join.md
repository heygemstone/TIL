>>join<<

SELECT DNAME, ENAME, JOB, SAL
       FROM EMP, DEPT
       WHERE EMP.DEPTNO = DEPT.DEPTNO AND
             EMP.JOB IN ('MANAGER','CLERK');
             --EMP테이블DEPTNO와 DEPT테이블DEPTNO가 같은 것 끼리 결합하라
             --JOIN의 조건 두개 서로다른 집합을 >>연관된 데이터<<를 기반으로 결합하라!
             
DESC EMP;
DESC DEPT;
--DEPTNO라는 공통된 COLUMN을 갖고있다.
DESC SALGRADE;
SELECT * FROM SALGRADE; --급여의 등급을 저장한 테이블
--SALGRADE 테이블은 EMP 테이블과 공통된 연결고리 (SAL) (LOSAL)(HISAL)으로 연결되어 있음

--**ERD 데이터베이스 설계도
--**UML 모델링 언어 개발툴이 아니라 모델링에 대한 개념.
--------Unified Modeling Language 통합 모델링 언어.
--검색해보기 그림으로 그리기: 직관적이고 알아보기 쉽다
--네트워크 망형 데이터베이스: 데이터가 물리적으로 연결되어있다.
--장점)빠르다 단점)복잡하고 유연성이 떨어진다.


SELECT DNAME, ENAME, JOB, SAL, EMP.DEPTNO, DEPT.DEPTNO
       FROM EMP, DEPT
       WHERE EMP.DEPTNO = DEPT.DEPTNO AND
             EMP.JOB IN ('MANAGER','CLERK');
             
SELECT * FROM DEPT;
SELECT * FROM EMP;
--한 개 이상의 TABLE로부터 DATA를 읽어야 할 때 JOIN이 필요하다.
--JOIN은 피할래야 피할수없는 필수현상이다
--RDB에 맞게 모델링을 했다면 JOIN은 피할수없다. 성능을 고려한 설계가 중요하다
--1)EQUI JOIN : EQUAL연산자를 이용해 JOIN한 것이다. 서로 일치하는 데이터를 기반으로 JOIN한다.
--EQUI JOIN = INNER JOIN
--2)NON EQUI JOIN : 값이 일치않은 연산자를 이용해 JOIN
--3)OUTER JOIN : 아웃사이더까지도 포용하는 JOIN, OUTSIDE까지 들어가는 JOIN 
--4)SELF JOIN: 자기가 자기자신과 JOIN하는 것이 SELFJOIN

--JOIN을 처리하는 내부 알고리즘 3가지

--1)NESTED LOOP JOIN 
--JOIN을 어떤 알고리즘으로 처리할건지 OPTIMIZER가 판단한다.
--FOR문 2개쓰는것처럼 처리하는방식
--INDEX를 사용한다
--2)SORT MERGE JOIN
--두테이블을 SORT해서 SORT한 결과를 JOIN함
--3)HASH JOIN
--SORT MERGE JOIN의 개선안이 HASH JOIN이다
--DISTINCT 중복제거도 원래 SORT MERGE를 사용해서 중복을 제거했는데 HASH로 바꿨음
--HASH를 이해해야함 DB DATA 암호화할때도 HASH가 쓰인다.
--왜? SQL, JOIN이 느려지므로 궁금해질 것이다.
--내가 보고싶은 DATA가 여러 테이블에 흩어져있다. 이걸 한꺼번에 보고 싶을때 JOIN을 사용한다.
--2번 SORT MERGE를 사용할경우 결과가 정렬된 상태로 나올 수도 있다.

--EQUI-JOIN (SIMPLE JOIN/INNER JOIN)
SELECT DNAME, ENAME, JOB, SAL FROM EMP, DEPT WHERE EMP.DEPTNO = DEPT.DEPTNO;

--실행순서 필터링을 먼저하고 ORDER BY를 한다 그래야 일의 양이 줄어드니까
--내가 ORACLE의 OPTIMIZER라면?
--SCHEMA : ~소유의 (스키마)

SELECT DNAME, ENAME, JOB, SAL FROM EMP, DEPT
       WHERE EMP.DEPTNO = DEPT.DEPTNO AND
             EMP.JOB IN ('MANAGER','CLERK')
             ORDER BY DNAME;
             
--4번
SELECT DEPT.DNAME, EMP.ENAME, EMP.JOB, EMP.SAL FROM C##03.EMP, C##03.DEPT
            WHERE EMP.DEPTNO = DEPT.DEPTNO;
--OBJECT NAME표기법 [SCHEMA.]OBJECT_NAME

--TABLE ALIAS 용도 1.편의성 2.가독성(의미있는 이름 사용) EX) EMP E, EMP A
--                 3.SELF JOIN시 필수!! 3. 동일 COLUMN명이 존재하는 경우
--1번 ALIAS 붙이기?? 별칭을 어디다가 붙이는거지?? FROM에다가??
SELECT D.DNAME, A.ENAME, A.JOB, A.SAL FROM EMP A, DEPT D 
                                      WHERE A.DEPTNO = D.DEPTNO;
--2.ANSI / ISO EQUI-JOIN
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL
                FROM EMP E INNER JOIN DEPT D
                ON E.DEPTNO = D.DEPTNO;  --ON이란 키워드를 써서 JOIN의 조건을 표기한다 (ANSI SQL)

--3번 ANSI
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL
       FROM EMP E INNER JOIN DEPT D
       ON E.DEPTNO = D.DEPTNO --JOIN조건 명시 헷갈리지 않음
       WHERE E.DEPTNO IN(10,20) AND D.DNAME = 'RESEARCH';
--FROM에 복수의 테이블이 들어감 

-----NON EQUI-JOIN
--EQUAL(=)이외의 연산자를 사용해 JOIN한다
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE 
               FROM EMP E, SALGRADE S
               WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
--여기선 BETWEEN 연산자를 사용해 JOIN함
SELECT DNAME, ENAME, JOB, SAL, GRADE
              FROM EMP E, DEPT D, SALGRADE S 
              WHERE E.DEPTNO = D.DEPTNO AND E.SAL 
              BETWEEN S.LOSAL AND S.HISAL;
--3개의 TABLE을 JOIN함, 최소 JOIN조건 명시: N(테이블 개수)-1
--JOIN의 조건은 WHERE절에 적는다??

--6번
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE, E.DEPTNO 
                FROM EMP E, SALGRADE S
                WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL AND E.DEPTNO IN(10,30)
                ORDER BY E.ENAME;
                --JOIN조건은 여러개로 줄수있는가??
--7번
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE, E.DEPTNO
               FROM EMP E, SALGRADE S
               WHERE E.SAL < S.LOSAL AND E.DEPTNO IN(10,30)
               ORDER BY E.ENAME;
--중복된 데이터가 반복해서 나옴 
--JOIN잘못하면 DUMMY DATA가나온다
--JOIN도 집합의 개념으로 이해한다. --곱집합처럼 나온다 

-->>OUTER-JOIN<< (OUT SIDER 정보를 보겠다는 뜻)
--JOIN 조건에 직접 만족하지 않는 정보도 조회한다.
--부족한 쪽에 (+)를해준다
--넘치는 쪽에 (+)를 해주면 아무 변화가 없음. EQUI JOIN과 아무런 차이가 없고 쓸데없는 OUTER JOIN이 된다
--8번 INNER JOIN
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL  
                FROM EMP E, DEPT D
                WHERE E.DEPTNO = D.DEPTNO
                ORDER BY D.DNAME;
--9번 INNER JOIN
SELECT D.DEPTNO, D.DNAME, E.ENAME, E.JOB, E.SAL
                FROM EMP E, DEPT D
                WHERE E.DEPTNO = D.DEPTNO
                ORDER BY D.DEPTNO;
--10번 OUTER JOIN
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL, D.DEPTNO 
                   FROM EMP E, DEPT D
                   WHERE E.DEPTNO(+) = D.DEPTNO
                   ORDER BY D.DNAME;
                   --여기서 나오는건 40번부서에 속한사람?? 그정보는 어디서갖고오지?
SELECT * FROM DEPT;
SELECT * FROM EMP;

--11
SELECT D.DEPTNO, D.DNAME, E.ENAME, E.JOB, E.SAL
                 FROM EMP E, DEPT D
                 WHERE E.DEPTNO(+) = D.DEPTNO
                 ORDER BY D.DEPTNO;
--12 EQUI JOIN과 결과가 동일, 효율성 떨어짐
--40번부서 안나옴
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL 
                 FROM EMP E, DEPT D
                 WHERE E.DEPTNO = D.DEPTNO(+)
                 ORDER BY D.DNAME;
                 
--13 NVL()NULL치환함수를 써서 정말 치환되었는지 확인하는 예제
SELECT D.DNAME, NVL(E.ENAME,'비상근부서'), E.JOB, E.SAL
                FROM EMP E, DEPT D
                WHERE E.DEPTNO(+) = D.DEPTNO
                ORDER BY D.DNAME;
--정말 NULL로 채우는가?? 40번부서 뜯어보기

--14 양뱡항 OUTER JOIN 오라클은 지원안함 ANSI는 지원함
SELECT D.DNAME, E.ENAME, E.JOB, E.SAL FROM EMP E, DEPT D
                WHERE E.DEPTNO(+) = D.DEPTNO(+)
                ORDER BY D.DNAME;
--오라클은 양방향 OUTER JOIN을 허용하지 않는다.
--SQL 1999 ANSI SQL을 뜻함

-->>요구) 아래 2개의 SQL을 실행하여 데이터를 비교한후 원하는 결과집합이 나오도록
--2번째 SQL을 수정하십시오
--a)

SELECT D.DEPTNO, D.DNAME, E.ENAME, E.JOB, E.SAL 
                 FROM EMP E, DEPT D
                 WHERE E.DEPTNO = D.DEPTNO AND E.SAL >2000
                 ORDER BY D.DNAME;
--b)
SELECT D.DEPTNO, D.DNAME, E.ENAME, E.JOB, E.SAL 
                 FROM EMP E, DEPT D
                 WHERE E.DEPTNO(+) = D.DEPTNO AND
                 E.SAL > 2000 OR SAL IS NULL
                 ORDER BY D.DNAME;
                 --E.SAL이 NULL이라서 인식하지 못한 것임
                 --NULL 값을 인식하고 OUTER JOIN을 만들어 줘야함
                 --40번부서의 OPERATIONS는 NULL값이 들어가 있으므로 조회하도록 조건을 줘야함
                 

--ANSI-SQL: LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN
--1 LEFT OUTER JOIN 
--14번까지만 나옴 40번 부서 정보표기 안됨 
SELECT E.DEPTNO, D.DNAME, E.ENAME
                 FROM C##03.EMP E 
                 LEFT OUTER JOIN C##03.DEPT D
                 ON E.DEPTNO = D.DEPTNO
                 ORDER BY E.DEPTNO;
--2 RIGHT OUTER JOIN DEPT를 기준 테이블(Driving Table)로 잡고 JOIN연산을 수행함
--15번까지 나옴
--40번 부서의 정보표기가 됨? E.DEPTNO에 나타나는 값 확인
SELECT E.DEPTNO, D.DNAME, E.ENAME
                 FROM C##03.EMP E RIGHT OUTER JOIN C##03.DEPT D
                 ON E.DEPTNO = D.DEPTNO
                 ORDER BY E.DEPTNO;
--3 DEPT를 DRIVING TABLE로 JOIN연산수행, 40번 부서 정보표기
--4 양방향 FULL OUTER JOIN
--역시 40번 부서 정보표기가 됨 
SELECT D.DEPTNO, D.DNAME, E.ENAME
                 FROM C##03.EMP E FULL OUTER JOIN C##03.DEPT D
                 ON E.DEPTNO = D.DEPTNO
                 ORDER BY E.DEPTNO;


-----------
--SELF JOIN
-------------
SELECT E.ENAME||' ''S MANAGER IS ' || M.ENAME
            FROM EMP E, EMP M
            WHERE E.MGR = M.EMPNO
            ORDER BY M.ENAME;
--위 정보에서 사장님 정보는 누락되었다. SQL을 수정하여 작성하라
--매니저가 없는 경우 매니저 이름은 NOBODY로 표기할것 
--EMPNO->MGR 선택적관계 // MGR->EMPNO 필수적관계 
--모든 매니저는 사원이어야 한다

------------------
--CARTESIAN PRODUCT--곱집합
------------------
 --정의 : 데카르트의 곱집합, 수학의 곱집합
 --원인 : 1) JOIN조건 생략시 2)잘못된 JOIN 조건
 --문제점 : 유용하지 않은 대량의 데이터를 생성함
 --용도 : 1) 테스트용 샘플 데이터를 생성
        --2) 곱집합 기능을 이용한 빠른 연산에 응용함
 
SELECT ENAME, JOB, DNAME FROM EMP, DEPT;

--2
SELECT ENAME, JOB, SAL, DNAME 
                        FROM EMP, DEPT 
                        WHERE EMP.SAL > 2000 AND DEPT.DEPTNO IN(10,20);

--3
SELECT ENAME, JOB, DNAME FROM EMP, DEPT
                         WHERE EMP.SAL > 2000 OR DEPT.DEPTNO IN (10,20);
                        --곱집합 실행
--4
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE FROM EMP E, SALGRADE S
                WHERE E.SAL < S.LOSAL AND E.DEPTNO IN(10,30)
                ORDER BY E.ENAME;
                
                
