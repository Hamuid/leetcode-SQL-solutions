# leetcode-SQL-solutions
SQL 기록 (feat.Leetcode)
--문제: all dates' id with higher temperatures compared to its previous dates (yesterday).
--Input: 
--Weather table:
--+----+------------+-------------+
--| id | recordDate | temperature |
--+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+

-- 방법1. SELF JOIN
-- select currentday.id
-- from weather as currentday
--     join weather as yesterday
--     on currentday.recordDate = yesterday.recordDate + 1
-- where currentday.temperature > yesterday.temperature

-- 방법2. EXISTS
-- select currentday.id
-- from weather as currentday
-- where exists (
--         select 1
--         from weather as yesterday
--         where currentday.temperature > yesterday.temperature
--             AND currentday.recordDate = yesterday.recordDate + 1
-- )

-- 방법3. LAG 함수
SELECT id
from (
    select id, recordDate, temperature,
        LAG(recordDate) OVER(order by recordDate) as pre_recordDate,
        LAG(temperature) OVER(order by recordDate) as pre_temperature
    from weather
)
where recordDate = pre_recordDate + 1
    AND temperature > pre_temperature
