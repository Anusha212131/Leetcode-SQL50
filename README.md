# Leetcode-SQL50

## Basic SELECT

#### 1.[1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
````sql
SELECT w.id AS Id 
FROM Weather AS w
JOIN Weather AS s
ON DATEDIFF(w.recordDate,s.recordDate) = 1 AND w.temperature > s.temperature
````
####  2. [584 - Find Customer Referee](https://leetcode.com/problems/find-customer-referee)
````sql
SELECT name
FROM Customer
WHERE referee_id <> 2 OR referee_id IS NULL
````
#### 3. [595 - Big Countries](https://leetcode.com/problems/big-countries/)
````sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000
````
#### 4. [1148 - Article Views I](https://leetcode.com/problems/article-views-i)
````sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id
````
#### 5. [1683 - Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
````sql
SELECT DISTINCT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15
````

## Basic Joins

#### 6. [1378 - Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier)
````sql
SELECT u.unique_id, e.name
FROM employees AS e
LEFT JOIN employeeuni AS u
ON e.id = u.id
````

#### 7. [1068 - Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
````sql
SELECT p.product_name, s.year, s.price
FROM Sales AS s
LEFT JOIN Product AS p
ON s.product_id = p.product_id
````

#### 8. [1581 - Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
````sql
SELECT v.customer_id, COUNT(v.visit_id) AS count_no_trans
FROM Visits AS v
LEFT JOIN Transactions AS t
ON v.visit_id = t.visit_id
WHERE v.visit_id NOT IN (SELECT visit_id FROM Transactions)
GROUP BY v.customer_id
````

#### 9. [197 - Rising Temperature](https://leetcode.com/problems/rising-temperature/)
````sql
SELECT w.id AS Id 
FROM Weather AS w
JOIN Weather AS s
ON DATEDIFF(w.recordDate,s.recordDate) = 1 AND w.temperature > s.temperature
````

#### 10.[1661 - Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
````sql
SELECT s.machine_id, ROUND(AVG(e.timestamp-s.timestamp),3) AS processing_time
FROM Activity AS s
JOIN Activity AS e
ON s.machine_id = e.machine_id AND s.process_id = e.process_id AND s.activity_type = 'start' AND e.activity_type = 'end'
GROUP BY s.machine_id
````

#### 11. [577 - Employee Bonus](https://leetcode.com/problems/employee-bonus/solutions/)
````sql
SELECT e.name, b.bonus
FROM Employee AS e
LEFT JOIN Bonus AS b
ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL
````

#### 12. [1280 - Students and Examinations](https://leetcode.com/problems/students-and-examinations/)
````sql
SELECT a.student_id, a.student_name, a.subject_name, 
SUM(CASE WHEN e.subject_name IS NOT NULL THEN 1 ELSE 0 END) AS attended_exams
FROM(SELECT s.*,su.*
FROM Students AS s
CROSS JOIN Subjects AS su)a
LEFT JOIN Examinations AS e
ON a.student_id = e.student_id AND a.subject_name = e.subject_name
GROUP BY a.student_id, a.student_name, a.subject_name
ORDER BY a.student_id, a.subject_name
````

#### 13. [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports)

````sql
SELECT e.name, b.bonus
FROM employee AS e
LEFT JOIN bonus AS b
ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL
````

#### 14. [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
````sql
SELECT s.user_id, ROUND(IFNULL(SUM(IF(c.action = 'confirmed',1,0))/COUNT(c.action),0),2) AS confirmation_rate 
FROM Signups AS s
LEFT JOIN Confirmations AS c
ON s.user_id = c.user_id
GROUP BY s.user_id 
````
## Basic Aggregate Functions

#### 15. [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies)
````sql
SELECT *
FROM Cinema
WHERE description <> 'boring' AND id % 2 <> 0
ORDER BY rating DESC
````

#### 16. [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)
````sql
SELECT p.product_id, 
ROUND(IFNULL(SUM(p.price*u.units)/SUM(u.units),0),2) AS average_price
FROM Prices AS p
LEFT JOIN UnitsSold AS u
ON p.product_id = u.product_id AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP .BY p.product_id
````

#### 17. [1075. Project Employees I](https://leetcode.com/problems/project-employees-i)
````sql
SELECT p.project_id,
ROUND(AVG(e.experience_years),2) AS average_years
FROM Project AS p
JOIN Employee AS e
ON p.employee_id = e.employee_id
GROUP BY p.project_id
````

#### 18. [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest)
````sql
SELECT contest_id, ROUND(COUNT(user_id)/(SELECT COUNT(user_id) FROM Users)*100,2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id 
````

#### 19. [1211 Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage)
````sql
SELECT query_name, ROUND(AVG(rating/position),2) AS quality,
ROUND(SUM(IF(rating<3,1,0))/COUNT(rating)*100,2) AS poor_query_percentage
FROM Queries
GROUP BY query_name
````
#### 20. [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)
````sql
SELECT
DATE_FORMAT(trans_date, '%Y-%m') AS month, country, COUNT(*) AS trans_count, SUM(IF(state = 'approved',1,0)) AS approved_count, SUM(amount) AS trans_total_amount,
SUM(IF(state = 'approved',amount,0)) AS approved_total_amount
FROM Transactions
GROUP BY month,country
````

#### 21. [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)
````sql
SELECT ROUND(SUM(IF(order_no = 1 AND order_date = customer_pref_delivery_date, 1, 0))/SUM(IF(order_no = 1,1,0))*100,2) AS immediate_percentage
FROM(SELECT order_date, customer_pref_delivery_date, 
ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date) as order_no
FROM delivery)a
````

#### 22. [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)
````sql
````
