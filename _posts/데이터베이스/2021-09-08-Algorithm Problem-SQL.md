### SQL note



```SQL
#오름차순 기본
SELECT * From ANIMAL_INS ORDER BY ANIMAL_ID;

#내림차순
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC;

#Where문
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION = "Sick" ORDER BY ANIMAL_ID;

SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION <> "Aged" ORDER BY ANIMAL_ID;

#DATETIME에 대해서는 내림차순
SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME, DATETIME DESC;

#상위 한개
SELECT NAME FROM ANIMAL_INS ORDER BY DATETIME LIMIT 1
```





```sql
#COUNT
SELECT COUNT(*) FROM ANIMAL_INS;

#중복제거
SELECT COUNT(DISTINCT NAME) FROM ANIMAL_INS;

```



```SQL
#GROUP BY
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) COUNT
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
order by ANIMAL_TYPE

#GROUP BY, HAVING
SELECT name, count(name) as COUNT
from animal_ins
group by name
having count > 1
order by name

#DATETIME 시간 조회
SELECT HOUR(DATETIME) AS HOUR, COUNT(*) AS COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR
HAVING HOUR >= 9 AND HOUR <= 19
ORDER BY HOUR

#
WITH RECURSIVE TIMETABLE(HOUR) AS (
    SELECT 0
    UNION
    SELECT TIMETABLE.HOUR + 1 FROM TIMETABLE WHERE TIMETABLE.HOUR < 23
)

SELECT HOUR, COUNT(A.ANIMAL_ID)
FROM TIMETABLE AS T LEFT JOIN ANIMAL_OUTS AS A ON T.HOUR = HOUR(A.DATETIME)
GROUP BY HOUR
ORDER BY HOUR
```



```SQL
SELECT ANIMAL_ID FROM ANIMAL_INS where NAME IS NULL

#if 문
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID

#한쪽에는 있지만 한쪽에는 없는..
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_OUTS A LEFT JOIN ANIMAL_INS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.ANIMAL_ID IS NULL
```



### JOIN



```SQL
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_OUTS A LEFT JOIN ANIMAL_INS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.DATETIME < B.DATETIME
ORDER BY B.DATETIME


SELECT A.NAME, A.DATETIME
FROM ANIMAL_INS A LEFT JOIN ANIMAL_OUTs B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.ANIMAL_ID IS NULL
ORDER BY A.DATETIME LIMIT 3


SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME
FROM ANIMAL_OUTS A LEFT JOIN ANIMAL_INS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.SEX_UPON_OUTCOME != B.SEX_UPON_INTAKE AND A.ANIMAL_ID IS NOT NULL 
ORDER BY A.ANIMAL_ID
```



### String, Date



```SQL
select animal_id, name, sex_upon_intake
from animal_ins
where name in ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty');

SELECT ANIMAL_ID, NAME 
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'Dog'AND UPPER(NAME) LIKE UPPER('%el%')
ORDER BY NAME

SELECT ANIMAL_ID, NAME,
CASE WHEN SEX_UPON_INTAKE LIKE '%Intact%' THEN 'X' ELSE 'O' END AS '중성화'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;

SELECT
    OUTS.ANIMAL_ID, OUTS.NAME
FROM
    ANIMAL_OUTS AS OUTS
RIGHT JOIN
    ANIMAL_INS AS INS
ON 
    INS.ANIMAL_ID = OUTS.ANIMAL_ID
ORDER BY
    DATEDIFF(OUTS.DATETIME, INS.DATETIME) DESC
LIMIT
    2;
    
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d')
FROM ANIMAL_INS
```





헤비 유저가 소유한 장소

```java
-- 코드를 입력하세요
-- SELECT ID, NAME, HOST_ID FROM PLACES GROUP BY HOST_ID having count(HOST_ID) > 1 ORDER BY ID
WITH IDTABLE ( ID, NAME, HOST_ID ) AS (
    SELECT A.ID, A.NAME, A.HOST_ID FROM PLACES A GROUP BY HOST_ID having count(HOST_ID) > 1 
)
select A.ID, A.NAME, A.HOST_ID
from PLACES A LEFT JOIN IDTABLE B
ON A.HOST_ID = B.HOST_ID
WHERE B.ID IS NOT NULL
ORDER BY A.ID
```



