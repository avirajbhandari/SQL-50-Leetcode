# SQL-50-Leetcode
Solutions for [SQL 50 Study Plan](https://leetcode.com/studyplan/sql50/) on LeetCode

---
## Select

#### [1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)

```sql
Select product_id
From Products
Where low_fats = 'Y' and recyclable = 'Y';
```

#### [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select name 
From Customer 
Where referee_id Not In(2) or referee_id is null;
```

#### [595. Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select name, population, area 
From World
Where area >= 3000000 or population >= 25000000;
```

#### [1148. Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;
```

#### [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select tweet_id 
From Tweets
Where CHAR_LENGTH(content) > 15; 
```
## Basic Joins

#### [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select name, unique_id 
From Employees a
Left Join EmployeeUNI b on a.id = b.id; 
```

#### [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select product_name, year, price
From Sales a
Join Product b on a.product_id = b.product_id;
```

#### [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select a.customer_id, COUNT(a.visit_id) As 'count_no_trans'
From Visits a
Left Join Transactions b on a.visit_id = b.visit_id
Where b.transaction_id Is Null
Group By a.customer_id;
```

#### [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT today.id
FROM Weather yesterday 
CROSS JOIN Weather today

WHERE DATEDIFF(today.recordDate,yesterday.recordDate) = 1
AND today.temperature > yesterday.temperature;
```

#### [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
# Write your MySQL query statement below
Select a1.machine_id, round(avg(a2.timestamp - a1.timestamp), 3) As 'processing_time'
From Activity a1
Join Activity a2 
On a1.machine_id = a2.machine_id And a1.process_id = a2.process_id And a1.activity_type ='start' and a2.activity_type ='end'
Group by a1.machine_id;
```

#### [577. Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select name, bonus
From Employee a
Left Join Bonus b on a.empID = b.empID
Where bonus < 1000 or Bonus is NULL;
```

#### [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select s.student_id, s.student_name, sub.subject_name, count(e.subject_name) as attended_exams
From Students s
Cross Join Subjects sub
Left Join Examinations e 
on s.student_id = e.student_id AND sub.subject_name = e.subject_name
Group by s.student_id, s.student_name, sub.subject_name
Order by s.student_id, sub.subject_name;
```

#### [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name
FROM Employee
WHERE id IN (
    SELECT managerId
    FROM Employee
    GROUP BY managerId
    HAVING COUNT(*) > 4);
```

#### [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte AS (
    SELECT user_id, 
           ROUND(SUM(CASE WHEN action = 'confirmed' THEN 1 ELSE 0 END) / COUNT(action), 2) AS confirmation_rate
    FROM Confirmations 
    GROUP BY user_id
)

SELECT s.user_id, 
       IFNULL(c.confirmation_rate, 0) AS confirmation_rate
FROM Signups s
LEFT JOIN cte c ON s.user_id = c.user_id
ORDER BY s.user_id;
```
## Basic Aggregate Functions

#### [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select id, movie, description, rating
From Cinema
Where id mod 2 <> 0 and description <> 'boring'
Order by rating desc;
```

#### [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.product_id, IFNULL(ROUND(SUM(units*price)/SUM(units),2),0) AS average_price
FROM Prices p LEFT JOIN UnitsSold u
ON p.product_id = u.product_id AND
u.purchase_date BETWEEN start_date AND end_date
group by product_id;
```

#### [1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select a.project_id, Round(avg(experience_years), 2) as 'average_years'
From Project a
Join Employee b on a.employee_id = b.employee_id
Group by project_id; 
```

#### [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select b.contest_id, Round(Count(Distinct b.user_id) *100/(Select Count(*) From Users), 2) as 'percentage'
From Register b
Group by b.contest_id
Order by percentage desc, b.contest_id asc;
```

#### [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    query_name, 
    ROUND(avg(a.rating / a.position), 2) AS quality, 
    ROUND(SUM(CASE WHEN a.rating < 3 THEN 1 ELSE 0 END) * 100 / count(*), 2) AS poor_query_percentage
FROM 
    Queries a
GROUP BY 
    query_name;
```

#### [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month, 
    country, 
    COUNT(*) AS trans_count, 
    COUNT(CASE WHEN state = 'approved' THEN 1 END) AS approved_count, 
    SUM(amount) AS trans_total_amount, 
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM 
    Transactions
GROUP BY 
    DATE_FORMAT(trans_date, '%Y-%m'), country;
```

#### [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select round(avg(order_date = customer_pref_delivery_date)*100, 2) as immediate_percentage
from Delivery
where (customer_id, order_date) in (
  Select customer_id, min(order_date) 
  from Delivery
  group by customer_id
);
```

#### [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT
  ROUND(COUNT(DISTINCT player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM
  Activity
WHERE
  (player_id, DATE_SUB(event_date, INTERVAL 1 DAY))
  IN (
    SELECT player_id, MIN(event_date) AS first_login FROM Activity GROUP BY player_id
  );
```

## Sorting and Grouping

#### [2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select teacher_id, count(distinct subject_id) as 'cnt'
From Teacher
Group by teacher_id;
```

#### [1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT activity_date AS day, 
       COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY activity_date;
```

#### [1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select product_id, year as first_year, quantity, price
from Sales
where (product_id, year) in (
    select product_id, min(year)
    from Sales
    group by product_id
);
```

#### [596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select class
From Courses
Group by class
Having count(student) >= 5;
```

#### [1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select user_id, Count(*) as 'followers_count'
From Followers
Group by user_id
Order by user_id; 
```

#### [619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
With cte AS
(
    Select num
    From MyNumbers
    Group by num
    Having Count(num) = 1
)

Select Max(num) as 'num'
From cte;
```

#### [1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT customer_id 
FROM Customer 
GROUP BY customer_id
HAVING COUNT(distinct product_key) = (SELECT COUNT(product_key) FROM Product);
```

## Advanced Select and Join

#### [1731. The Number of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte AS (
    SELECT 
        reports_to, 
        COUNT(employee_id) AS reports_count, 
        ROUND(AVG(age), 0) AS average_age
    FROM employees
    WHERE reports_to IS NOT NULL
    GROUP BY reports_to
)
SELECT 
    c.reports_to AS employee_id, 
    e.name, 
    c.reports_count,
    c.average_age
FROM cte c
LEFT JOIN employees e ON c.reports_to = e.employee_id
ORDER BY employee_id;
```
#### [1789. Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag='Y' OR 
    employee_id in
    (SELECT employee_id
    FROM Employee
    Group by employee_id
    having count(employee_id)=1);
```

#### [610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT x, y, z,
       CASE 
           WHEN x + y <= z THEN 'No'
           WHEN y + z <= x THEN 'No'
           WHEN x + z <= y THEN 'No'
           ELSE 'Yes'
       END AS Triangle
FROM Triangle;
```

#### [180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
with cte as (
    select num,
    lead(num,1) over() num1,
    lead(num,2) over() num2
    from logs

)

select distinct num ConsecutiveNums from cte where (num=num1) and (num=num2);
```

#### [1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select product_id,10 as price from Products where product_id not in (select product_id from Products where change_date <= '2019-08-16')
union 
select product_id,new_price as price from Products where (product_id,change_date) in (select product_id,max(change_date) from Products where change_date <= '2019-08-16' group by product_id);
```

#### [1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH RankedQueue AS (
    SELECT 
        turn, 
        weight, 
        SUM(weight) OVER (ORDER BY turn) AS cumulative_weight, #It calculates the running total of weight, ordered by turn
        person_name
    FROM Queue
)
SELECT person_name
FROM RankedQueue
WHERE cumulative_weight <= 1000
ORDER BY turn DESC
LIMIT 1;
```

#### [1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select
    'Low Salary' as category,
    SUM(income<20000) as accounts_count
From Accounts

Union
    Select 'Average Salary' as category, 
    SUM(income between 20000 and 50000) as accounts_count
    From Accounts
Union 
    Select 'High Salary' as category, 
    SUM(income>50000) as accounts_count
    From Accounts;
```

## Subqueries

#### [1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000 
  AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```

#### [626. Exchange Seats](https://leetcode.com/problems/exchange-seats/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    CASE 
        WHEN id = (select max(id) from seat) and id % 2 = 1 then id 
        WHEN id % 2 = 1 THEN id + 1 else id-1 end as id, 
        student
FROM Seat
ORDER BY id;
```

#### [Movie Rating](https://leetcode.com/problems/movie-rating/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte1 AS (
    SELECT m.movie_id, r.user_id, m.title, r.rating, r.created_at
    FROM Movies m
    JOIN MovieRating r ON m.movie_id = r.movie_id
    WHERE r.created_at BETWEEN '2020-02-01' AND '2020-02-28'
),
cte2 AS (
    SELECT m.movie_id, r.user_id, m.title, r.rating, r.created_at
    FROM Movies m
    JOIN MovieRating r ON m.movie_id = r.movie_id
)

-- Most Active User
SELECT 
    (SELECT u.name
     FROM cte2
     JOIN Users u ON cte2.user_id = u.user_id
     GROUP BY u.name
     ORDER BY COUNT(cte2.user_id) DESC, u.name ASC
     LIMIT 1) AS results

UNION ALL

-- Highest Rated Movie with Tie Handling (lexicographical order if avg ratings are the same)
SELECT 
    (SELECT cte1.title
     FROM cte1
     GROUP BY cte1.movie_id
     ORDER BY AVG(cte1.rating) DESC, cte1.title ASC
     LIMIT 1) AS results;
```

#### [1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
With cte as
(Select visited_on, SUM(amount) as total_amount
From Customer
Group By visited_on),

cte2 as
(Select visited_on, SUM(total_amount) OVER (Order by visited_on ROWS Between 6 PRECEDING AND CURRENT ROW) as amount,  ROUND(AVG(total_amount) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW), 2) AS average_amount
From cte)

Select *
From cte2 
Where visited_on >= (Select visited_on from cte2 Order by visited_on LIMIT 1) + 6
Order by visited_on;
```

#### [602. Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte1 AS (
    SELECT accepter_id AS id, COUNT(*) AS c
    FROM RequestAccepted
    GROUP BY accepter_id

    UNION ALL

    SELECT requester_id AS id, COUNT(*) AS c
    FROM RequestAccepted
    GROUP BY requester_id
)
SELECT id, SUM(c) as num  -- Summing counts for the same id
FROM cte1 
GROUP BY id  -- Ensuring each user has only one row
ORDER BY num DESC
LIMIT 1;
```

#### [585. Investments in 2016](https://leetcode.com/problems/investments-in-2016/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH unique_locations AS (
    -- Step 1: Keep policies where (lat, lon) is unique
    SELECT pid, tiv_2015, tiv_2016
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
),
duplicate_tiv AS (
    -- Step 2: Find tiv_2015 values that appear at least twice
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) >= 2
)

-- Step 3: Sum tiv_2016 for policies that meet both conditions
SELECT ROUND(SUM(ul.tiv_2016), 2) AS tiv_2016
FROM unique_locations ul
JOIN duplicate_tiv dt ON ul.tiv_2015 = dt.tiv_2015;
```

#### [185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte AS (
    SELECT e.id, e.name AS Employee, d.name AS Department, e.salary AS Salary
    FROM Employee e
    JOIN Department d ON e.departmentID = d.id
),
RankedSalaries AS (
    SELECT *, DENSE_RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS rnk
    FROM cte
)
SELECT Employee, Department, Salary
FROM RankedSalaries
WHERE rnk <= 3
ORDER BY Department, Salary DESC;
```

## Advanced String Functions / Regex / Clause

#### [1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select 
    user_id, 
    CONCAT(Upper(Substring(name,1,1)),
    Lower(Substring(name, 2, Length(name)))
    ) As name
From Users
Order by user_id;
```

#### [1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions like 'DIAB1%' or conditions like '% DIAB1%';
```

#### [196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
delete p1 
from person p1,person p2 
where p1.email=p2.email and p1.id>p2.id;
```

#### [176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    (CASE 
        WHEN COUNT(DISTINCT salary) >= 2 
        THEN (SELECT DISTINCT salary 
              FROM Employee 
              ORDER BY salary DESC 
              LIMIT 1 OFFSET 1) 
        ELSE NULL 
    END) AS SecondHighestSalary
FROM Employee;
```

#### [1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
Select sell_date, Count(DISTINCT product) as num_sold, 
Group_concat( Distinct product order by product ASC separator ',') as products
From Activities
Group by sell_date
Order by sell_date;
```
#### [1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.product_name, SUM(o.unit) AS unit
FROM Products p
JOIN Orders o ON p.product_id = o.product_id
WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'  -- Use BETWEEN for date ranges and correct end date
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100;
```

#### [1517. Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
# Write your MySQL query statement below
SELECT user_id, name, mail
FROM Users
WHERE mail LIKE CONCAT('%', '@leetcode.com')
AND mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@leetcode.com$';

/*^[A-Za-z]: Ensures the prefix of the email (before @) starts with a letter, which can be either uppercase or lowercase.
[A-Za-z0-9_.-]*: Allows the rest of the prefix to contain letters (uppercase or lowercase), digits, underscores, periods, or dashes.
@leetcode\.com$: Ensures that the email address ends with @leetcode.com.*/
```

