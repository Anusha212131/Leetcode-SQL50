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
