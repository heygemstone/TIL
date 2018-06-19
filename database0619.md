SELECT * FROM CUSTOMER;

SELECT * FROM CUSTOMER WHERE ADDRESS1 LIKE '경기 성남시%';

SELECT NAME, ADDRESS1, BIRTH_DT, GENDER, CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '6등급'
                      END AS CREDIT   
                      FROM CUSTOMER
                      WHERE (SUBSTR(BIRTH_DT,1,1) = '9') AND
                      ADDRESS1 LIKE '경기 성남시%'
                      ORDER BY GENDER;

SELECT ENAME, SYSDATE, HIREDATE FROM EMP;                     
                      
--변수바꿔보기 시각화프로젝트용
--퍼센테이지 그림이쁘게(아래 쿼리)
SELECT NAME, ADDRESS1, BIRTH_DT, GENDER, CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 2500 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 1200 THEN '4등급'
                      ELSE '5등급'
                      END AS CREDIT   
                      FROM CUSTOMER
                      WHERE (SUBSTR(BIRTH_DT,1,1) = '9') AND
                      ADDRESS1 LIKE '경기 성남시%'
                      ORDER BY GENDER;
                      
                      
--엑셀연동버전으로하나만들기
--얘는 자바버전
SELECT NAME, ADDRESS1, BIRTH_DT, GENDER, CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '6등급'
                      END AS CREDIT   
                      FROM CUSTOMER
                      WHERE FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)<30 
                      AND FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)>=20 AND
                      ADDRESS1 LIKE '경기 성남시%'
                      ORDER BY GENDER;
                      
                      
DESC CUSTOMER;                    
select * from customer;                    
SELECT * FROM CUSTOMER WHERE CREDIT_LIMIT >= 5000;         

SELECT ACCOUNT_MGR, NAME, ADDRESS1, CASE
                       WHEN CREDIT_LIMIT < 1000 THEN '신용불량자'
                       END AS 신용관리대상자
FROM CUSTOMER 
WHERE ADDRESS1 LIKE '경기 성남시%'
ORDER BY ACCOUNT_MGR; 


--엑셀연동버전 만나이계산안함
SELECT ACCOUNT_MGR, COUNT(*) AS 신용관리대상자수 FROM CUSTOMER    
WHERE CREDIT_LIMIT < 1000 AND
      (SUBSTR(BIRTH_DT,1,1) = '9') AND
      ADDRESS1 LIKE '경기 성남시%'  
GROUP BY ACCOUNT_MGR;

--자바연동버전 만나이로계산함 
SELECT ACCOUNT_MGR, COUNT(*) AS 신용관리대상자수 FROM CUSTOMER    
WHERE CREDIT_LIMIT < 1000 AND
      FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)<30 
                      AND FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)>=20 AND
      ADDRESS1 LIKE '경기 성남시%'  
GROUP BY ACCOUNT_MGR;




SELECT ACCOUNT_MGR, COUNT(*) AS 신용관리대상자수 FROM CUSTOMER    
WHERE CREDIT_LIMIT < 1000 AND
      ADDRESS1 LIKE '경기 성남시%'  
GROUP BY ACCOUNT_MGR;
                       
                       
                       
SELECT * FROM CUSTOMER;

SELECT CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '신용불량자'
                      END AS CREDIT   
                      FROM CUSTOMER;
//이게 진짜쿼리
SELECT 이십대, CREDIT, 성남,  count(*) FROM (SELECT CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '6등급'
                      END AS CREDIT,
                      CASE WHEN (SUBSTR(BIRTH_DT,1,1) = '9')
                      THEN '이십대' END AS 이십대,
                      CASE WHEN ADDRESS1 LIKE '경기 성남시%' 
                      THEN '성남시 거주' END AS 성남 
                      FROM CUSTOMER)
                      WHERE 이십대 IS NOT NULL AND 성남 IS NOT NULL
                      GROUP BY 이십대, CREDIT, 성남
                      ORDER BY CREDIT;
                      
                      
                      
--진짜쿼리에서 나이계산 다른방식으로해보기 자바버전
SELECT 이십대, CREDIT, 성남,  count(*) FROM (SELECT CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '6등급'
                      END AS CREDIT,
                      CASE 
                      WHEN FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)<30 
                      AND FLOOR(MONTHS_BETWEEN(sysdate, to_date(BIRTH_DT))/12)>=20 
                      THEN '이십대' END AS 이십대,
                      CASE WHEN ADDRESS1 LIKE '경기 성남시%' 
                      THEN '성남시 거주' END AS 성남 
                      FROM CUSTOMER)
                      WHERE 이십대 IS NOT NULL AND 성남 IS NOT NULL
                      GROUP BY 이십대, CREDIT, 성남
                      ORDER BY CREDIT;
                      
//버전3 진짜쿼리 퍼센테이지 조절
SELECT 이십대, CREDIT, 성남,  count(*) FROM (SELECT CASE  
                      WHEN CREDIT_LIMIT >= 4800 THEN '1-2등급'
                      WHEN CREDIT_LIMIT >= 2800 THEN '3-5등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '6-8등급'
                      ELSE '9-10등급'
                      END AS CREDIT,
                      CASE WHEN (SUBSTR(BIRTH_DT,1,1) = '9')
                      THEN '이십대' END AS 이십대,
                      CASE WHEN ADDRESS1 LIKE '경기 성남시%' 
                      THEN '성남시 거주' END AS 성남 
                      FROM CUSTOMER)
                      WHERE 이십대 IS NOT NULL AND 성남 IS NOT NULL
                      GROUP BY 이십대, CREDIT, 성남
                      ORDER BY CREDIT;
//버전3 진짜쿼리 퍼센테이지 조절 단계나누기 
SELECT 이십대, CREDIT, 성남,  count(*) FROM (SELECT CASE  
                      WHEN CREDIT_LIMIT >= 4950 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4800 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 4200 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 3200 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 2800 THEN '5등급'
                      WHEN CREDIT_LIMIT >= 2200 THEN '6등급'
                      WHEN CREDIT_LIMIT >= 1800 THEN '7등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '8등급'
                      WHEN CREDIT_LIMIT >= 400 THEN '9등급'
                      ELSE '10등급'
                      END AS CREDIT,
                      CASE WHEN (SUBSTR(BIRTH_DT,1,1) = '9')
                      THEN '이십대' END AS 이십대,
                      CASE WHEN ADDRESS1 LIKE '경기 성남시%' 
                      THEN '성남시 거주' END AS 성남 
                      FROM CUSTOMER)
                      WHERE 이십대 IS NOT NULL AND 성남 IS NOT NULL
                      GROUP BY 이십대, CREDIT, 성남
                      ORDER BY CREDIT;
                      
                      SELECT NAME, ADDRESS1, BIRTH_DT, GENDER, CASE  
                      WHEN CREDIT_LIMIT >= 5000 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4000 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 3000 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 2000 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '5등급'
                      ELSE '신용불량자'
                      END AS CREDIT   
                      FROM CUSTOMER
                      WHERE (SUBSTR(BIRTH_DT,1,1) = '9' OR SUBSTR(BIRTH_DT,1,2) = '89') AND
                      ADDRESS1 LIKE '경기 성남시%'
                      ORDER BY GENDER;
        
                      SELECT * FROM EMP WHERE ENAME LIKE '%\_%';
                      SEKECT * FROM EMP WHERE ENAME LIKE '%A';
     --수정용
        SELECT NAME, ADDRESS1, BIRTH_DT, GENDER, CASE  
                      WHEN CREDIT_LIMIT >= 4950 THEN '1등급'
                      WHEN CREDIT_LIMIT >= 4800 THEN '2등급'
                      WHEN CREDIT_LIMIT >= 4200 THEN '3등급'
                      WHEN CREDIT_LIMIT >= 3200 THEN '4등급'
                      WHEN CREDIT_LIMIT >= 2800 THEN '5등급'
                      WHEN CREDIT_LIMIT >= 2200 THEN '6등급'
                      WHEN CREDIT_LIMIT >= 1800 THEN '7등급'
                      WHEN CREDIT_LIMIT >= 1000 THEN '8등급'
                      WHEN CREDIT_LIMIT >= 400 THEN '9등급'
                      ELSE '10등급'
                      END AS CREDIT   
                      FROM CUSTOMER
                      WHERE (SUBSTR(BIRTH_DT,1,1) = '9') AND
                      ADDRESS1 LIKE '경기 성남시%'
                      ORDER BY GENDER;              
