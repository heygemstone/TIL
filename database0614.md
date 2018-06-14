<h3>1.문자열 합성, 결합 연산자를 통해 문장을 만들 수 있다.</h3>
column alias 목적 두가지
1. 가독성 2. 컨트롤 

우리가 쓰게 될 거의 모든 데이터의 형태는 숫자, 문자, 날짜이다.

<b>DATA TYPE CONVERSION</b>
-IMPLICIT D.T.C 
 코드가 명료하지 않음 / 어느날 버전이 바뀐다면?(현재 oracle 12c)
-EXPLICIT D.T.C EX)T0_CHAR(SAL) || '00'; 

둘 중에 명시적인게 좋다. (코드의 명료성)

숫자-> 문자
to_char
문자-> 숫자
to_number
날짜-> 문자
to_char
문자-> 날짜
to_date
날짜-> 숫자, 숫자-> 날짜: 없다
날짜는 표리부동 문자처럼 보이지만 dbms내에서는 숫자로 저장되어있다.
그래서 산술연산이 가능하다 날짜도 연산이 가능 그래서 날짜를 숫자로 변할 이유가 없다. 

1.CSV 형태 조회, TAB 용도 설명하기 
고객데이터를 분석해서 EXCEL로 연동하기, EXCEL로 시각화 하기 
주제는 자율주제 고객의 무엇을 분석할지는 알아서 정하세요.
빅데이터 분석 기법 중 하나.
1)탐색적 분석(본능적으로) 어디 쓰려고? PORTFOLIO
2.RDB 정의, 특징, 종류

DBMS에저장된 형식과 외부에 보여지는 형식이 다르다
누가 연산을 할것이냐? DBMS서버가 할거냐 클라이언트가 할거냐

데이터 날짜를 체크하는 경우도 많다.
날짜&시간
1) check (client application내부에서 수행, 효율적)
2) server (client마다 현지시간이 다를때, 공통 시간이 필요할 때), 
주문,접수 등 날짜&시간은 자주 필요하다.

sys와 sys계정 - 리눅스의 root계정과 비슷

<h3>2.NULL</H3>
null 신입도 경력사원도 실수를 많이 하는 부분이다.
잘 챙기세요. (실수도 그냥 실수가 아니에요..)
사전적의미: 값이 정의되지 않음. 존재하지 않는
unavailable, unassigned, unknown, inapplicable 
NOT THE SAME AS ZERO OR A BLANK SPACE(중요!)
*ASCII 코드 0:48 ' ':32 NULL:00*/

연산불가 / 비교불가 / 적용불가 

DBMS에서 데이터를 처리할 때 항상 고민해야한다 null이 있지 않을까? 

아래 5개의 SQL에 대해 설명 하십시요 
SELECT COUNT(*) FROM SCOTT.EMP;        
SELECT COUNT(*) FROM SCOTT.EMP WHERE  1 = 1; 
SELECT COUNT(*) FROM SCOTT.EMP WHERE  EMPNO = EMPNO;   
SELECT COUNT(*) FROM SCOTT.EMP WHERE  MGR = MGR;   SELECT COUNT(*) FROM SCOTT.EMP WHERE  COMM = COMM;   
 
 
아래 1개의 SQL에 대해 설명 하십시요 SELECT SAL,COMM,NVL(COMM,SAL),nvl2(COMM,SAL,0), NULLIF(JOB,'MANAGER') FROM emp;

ORDER BY 정렬의 방향이 중요하다

DEFAULT ACCENDING ORDER 오름차순이 DEFAULT값임
명시되지 않으면 기본값을 사용하라는 의미가 디폴트
ORDER BY 는 SELECT문장의 가장 마지막에 위치하고, 가장 마지막에 실행이 된다.
이걸 증명해주세요.

[정의]정렬(SORTING) 
[기준]정렬시 값 
비교기준 

숫자: 작은수/큰수     
EX)  123 < 456  

문자: 알파벳 순서(ASCII)  
EX)  ‘SCOTT’ < ‘T’ 날짜  ~ 숫자와 동일   
해당문자열의 가장 앞에 위치한 알파벳을 가지고 비교함

EX)  ‘2003/11/16’ > ‘19990916’ NULL ~ 가장 큰 값으로 간주 
 
[방향]
ASC  ~ 오름차순(ASCENDING ORDER), 
DEFAULT DESC ~ 내림차순(DESCENDING ORDER) 

NULL이 가장 아래쪽에 있음 ACCENDING ORDER 오름차순 작은거부터 큰값
NULL은 가장 큰값이 아니다. 가장 큰 값으로 간주하는 것.

ORDER BY뒤에 COLUMN이 온다. 첫번째 COLUMN이 기준이된다. 
 
