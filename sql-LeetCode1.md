## Big Countries

There is a table **World**

```
+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
```

A country is big if it has an area of bigger than 3 million square km or a population of more than 25 million.

Write a SQL solution to output big countries' name, population and area.

For example, according to the above table, we should output:

```
+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+
```

> 解法

``` sql
SELECT name, population, area FROM world where area > 3000000 OR population > 25000000
```

## Shortest Distance in a Line

Table **point** holds the x coordinate of some points on x-axis in a plane, which are all integers.

Write a query to find the shortest distance between two points in these points.

| x   |
|-----|
| -1  |
| 0   |
| 2   |

The shortest distance is '1' obviously, which is from point '-1' to '0'. So the output is as below:

| shortest|
|---------|
| 1       |

**Note**: Every point is unique, which means there is no duplicates in table point.

> 解法

```sql
Select min(p2.x-p1.x) as shortest
From point p1 , point p2 where p1.x<p2.x;
```

## Swap Salary

Given a table salary, such as the one below, that has m=male and f=female values. Swap all f and m values (i.e., change all f values to m and vice versa) with a single update query and no intermediate temp table.

For example:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

After running your query, the above salary table should have the following rows:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

``` sql
UPDATE salary SET sex = case sex 
WHEN 'm' THEN 'f' 
ELSE 'm' 
END;
```

## Not Boring Movies

X city opened a new cinema, many people would like to go to this cinema. The cinema also gives out a poster indicating the movies’ ratings and descriptions.

Please write a SQL query to output movies with an odd numbered ID and a description that is not 'boring'. Order the result by rating.

For example, table **cinema**:

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |
+---------+-----------+--------------+-----------+
```

For the example above, the output should be:

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |
+---------+-----------+--------------+-----------+
```

> 解法

``` sql
select * from cinema where mod(id,2) = 1 and description != 'boring' order by rating desc;
```

> tips

``` sql
mod(id,2) 用于取余
```

## Find Customer Referee

Given a table customer holding customers information and the referee.

```
+------+------+-----------+
| id   | name | referee_id|
+------+------+-----------+
|    1 | Will |      NULL |
|    2 | Jane |      NULL |
|    3 | Alex |         2 |
|    4 | Bill |      NULL |
|    5 | Zack |         1 |
|    6 | Mark |         2 |
+------+------+-----------+
```

Write a query to return the list of customers NOT referred by the person with id '2'.

For the sample data above, the result is:

```
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```

> 解法

``` sql
Select name from customer where referee_id is null or referee_id not in ('2');
```

## Triangle Judgement

A pupil Tim gets homework to identify whether three line segments could possibly form a triangle.

However, this assignment is very heavy because there are hundreds of records to calculate.
Could you help Tim by writing a query to judge whether these three sides can form a triangle, assuming table **triangle** holds the length of the three sides x, y and z.

| x  | y  | z  |
|----|----|----|
| 13 | 15 | 30 |
| 10 | 20 | 15 |

For the sample data above, your query should return the follow result:

| x  | y  | z  | triangle |
|----|----|----|----------|
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |

``` sql
select *, (case when x+y>z and y+z>x and x+z>y  then 'Yes'  Else 'No' END)  as triangle
From triangle ;
```

## Consecutive Available Seats

Several friends at a cinema ticket office would like to reserve consecutive available seats.
Can you help to query all the consecutive available seats order by the seat_id using the following cinema table?

| seat_id | free |
|---------|------|
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
Your query should return the following result for the sample case above.

| seat_id |
|---------|
| 3       |
| 4       |
| 5       |

``` sql
select distinct a.seat_id
from cinema a join cinema b
  on abs(a.seat_id - b.seat_id) = 1
  and a.free = true and b.free = true
order by a.seat_id
;
```

## Customer Placing the Largest Number of Orders

Query the **customer_number** from the **orders** table for the customer who has placed the largest number of orders.

It is guaranteed that exactly one customer will have placed more orders than any other customer.

The **orders** table is defined as follows:

| Column            | Type      |
|-------------------|-----------|
| order_number (PK) | int       |
| customer_number   | int       |
| order_date        | date      |
| required_date     | date      |
| shipped_date      | date      |
| status            | char(15)  |
| comment           | char(200) |

**Sample Input**

| order_number | customer_number | order_date | required_date | shipped_date | status | comment |
|--------------|-----------------|------------|---------------|--------------|--------|---------|
| 1            | 1               | 2017-04-09 | 2017-04-13    | 2017-04-12   | Closed |         |
| 2            | 2               | 2017-04-15 | 2017-04-20    | 2017-04-18   | Closed |         |
| 3            | 3               | 2017-04-16 | 2017-04-25    | 2017-04-20   | Closed |         |
| 4            | 3               | 2017-04-18 | 2017-04-28    | 2017-04-25   | Closed |         |
Sample Output

| customer_number |
|-----------------|
| 3               |
Explanation

**The customer with number '3' has two orders, which is greater than either customer '1' or '2' because each of them  only has one order. 
So the result is customer_number '3'.**

> 解法

``` sql
Select  customer_number from orders group by customer_number order by count(customer_number) desc limit 1;
```

## Sales Person

#### Description

Given three tables: **salesperson**, **company**, **orders**.
Output all the **names** in the table **salesperson**, who didn’t have sales to company 'RED'.

Example
Input

Table: **salesperson**

```
+----------+------+--------+-----------------+-----------+
| sales_id | name | salary | commission_rate | hire_date |
+----------+------+--------+-----------------+-----------+
|   1      | John | 100000 |     6           | 4/1/2006  |
|   2      | Amy  | 120000 |     5           | 5/1/2010  |
|   3      | Mark | 65000  |     12          | 12/25/2008|
|   4      | Pam  | 25000  |     25          | 1/1/2005  |
|   5      | Alex | 50000  |     10          | 2/3/2007  |
+----------+------+--------+-----------------+-----------+
```

The table **salesperson** holds the salesperson information. Every salesperson has a **sales_id** and a **name**.


Table: **company**

```
+---------+--------+------------+
| com_id  |  name  |    city    |
+---------+--------+------------+
|   1     |  RED   |   Boston   |
|   2     | ORANGE |   New York |
|   3     | YELLOW |   Boston   |
|   4     | GREEN  |   Austin   |
+---------+--------+------------+
```

The table **company** holds the company information. Every company has a com_id and a name.

Table: **orders**

```
+----------+----------+---------+----------+--------+
| order_id |  date    | com_id  | sales_id | amount |
+----------+----------+---------+----------+--------+
| 1        | 1/1/2014 |    3    |    4     | 100000 |
| 2        | 2/1/2014 |    4    |    5     | 5000   |
| 3        | 3/1/2014 |    1    |    1     | 50000  |
| 4        | 4/1/2014 |    1    |    4     | 25000  |
+----------+----------+---------+----------+--------+
```
The table **orders** holds the sales record information, salesperson and customer company are represented by sales_id and com_id.
output

```
+------+
| name | 
+------+
| Amy  | 
| Mark | 
| Alex |
+------+
```

Explanation

According to order '3' and '4' in table **orders**, it is easy to tell only salesperson 'John' and 'Alex' have sales to company 'RED',
so we need to output all the other names in table **salesperson**.

解法：

``` sql
Select s.name from salesperson s where  s.sales_id  not  in 
(select  o.sales_id  from orders o
Join company c on o.com_id=c.com_id where c.name='RED');
```

## Employee Bonus

Select all employee's name and bonus whose bonus is < 1000.

Table:**Employee**

```
+-------+--------+-----------+--------+
| empId |  name  | supervisor| salary |
+-------+--------+-----------+--------+
|   1   | John   |  3        | 1000   |
|   2   | Dan    |  3        | 2000   |
|   3   | Brad   |  null     | 4000   |
|   4   | Thomas |  3        | 4000   |
+-------+--------+-----------+--------+
```

empId is the primary key column for this table.

Table: **Bonus**

```
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
```

empId is the primary key column for this table.
Example ouput:

```
+-------+-------+
| name  | bonus |
+-------+-------+
| John  | null  |
| Dan   | 500   |
| Brad  | null  |
+-------+-------+
```

> 解法


``` sql
Select e.name, b.bonus from Employee e
Left join bonus b on e.empid=b.empid
Where b.bonus is null or b.bonus <1000;
```

## Duplicate Emails

Write a SQL query to find all duplicate emails in a table named **Person**.

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

For example, your query should return the following for the above table:

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**Note**: All emails are in lowercase.

> 解法

``` sql
select email from person   group by email having count(email) >=2;
```

## Combine Two Tables

Table: **Person**

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+

PersonId is the primary key column for this table.
```


Table: **Address**

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.

```
Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

FirstName, LastName, City, State

``` sql
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;

```

## Shortest Distance in a Plane

Table point_2d holds the coordinates (x,y) of some unique points (more than two) in a plane.

Write a query to find the shortest distance between these points rounded to 2 decimals.

| x  | y  |
|----|----|
| -1 | -1 |
| 0  | 0  |
| -1 | -2 |

The shortest distance is 1.00 from point (-1,-1) to (-1,2). So the output should be:

| shortest |
|----------|
| 1.00     |

> 解法

``` sql
select ROUND(SQRT(min((p1.x-p2.x)*(p1.x-p2.x)+(p1.y-p2.y)*(p1.y-p2.y))),2) as shortest
from point_2d p1,point_2d p2
where p1.x != p2.x or p1.y != p2.y;
```

## 181. Employees Earning More Than Their Managers


The **Employee** table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

Given the **Employee** table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

> 解法

``` sql
select name as 'Employee' from (
    select a.name, a.salary ,b.name as 'bname',b.salary as 'bsalary' from employee a
      left join employee b on a.managerid=b.id) aa
      where salary > bsalary
```

## 619. Biggest Single Number

Table **number** contains many numbers in column num including duplicated ones.

Can you write a SQL query to find the biggest number, which only appears once.

```
+---+
|num|
+---+
| 8 |
| 8 |
| 3 |
| 3 |
| 1 |
| 4 |
| 5 |
| 6 | 
```

For the sample data above, your query should return the following result:

```
+---+
|num|
+---+
| 6 |
```

**Note:**

If there is no such number, just output **null**.

``` sql
select ifnull((select num from (select num  
from number
group by num
having count(num) =  1
order by num desc 
limit 1
) as maxnum)  , null) as num
```


## 597. Friend Requests I: Overall Acceptance Rate

In social network like Facebook or Twitter, people send friend requests and accept others’ requests as well. Now given two tables as below:

Table: **friend_request**

| sender_id | send_to_id |request_date|
|-----------|------------|------------|
| 1         | 2          | 2016_06-01 |
| 1         | 3          | 2016_06-01 |
| 1         | 4          | 2016_06-01 |
| 2         | 3          | 2016_06-02 |
| 3         | 4          | 2016-06-09 |

Table: **request_accepted**

| requester_id | accepter_id |accept_date |
|--------------|-------------|------------|
| 1            | 2           | 2016_06-03 |
| 1            | 3           | 2016-06-08 |
| 2            | 3           | 2016-06-08 |
| 3            | 4           | 2016-06-09 |
| 3            | 4           | 2016-06-10 |

Write a query to find the overall acceptance rate of requests rounded to 2 decimals, which is the number of acceptance divide the number of requests.

For the sample data above, your query should return the following result.

|accept_rate|
|-----------|
|       0.80|

**Note:**

- The accepted requests are not necessarily from the table **friend\_request**. In this case, you just need to simply count the total accepted requests (no matter whether they are in the original requests), and divide it by the number of requests to get the acceptance rate.
- It is possible that a sender sends multiple requests to the same receiver, and a request could be accepted more than once. In this case, the ‘duplicated’ requests or acceptances are only counted once.
If there is no requests at all, you should return 0.00 as the accept_rate.
- Explanation: There are 4 unique accepted requests, and there are 5 requests in total. So the rate is 0.80.

**Follow-up:**

- Can you write a query to return the accept rate but for every month?
- How about the cumulative accept rate for every day?

> 解法

``` sql
select
round(
    ifnull(
    (select count(*) from (select distinct requester_id, accepter_id from request_accepted) as A)
    /
    (select count(*) from (select distinct sender_id, send_to_id from friend_request) as B),
    0)
, 2) as accept_rate;
```

## 183. Customers Who Never Order

Suppose that a website contains two tables, the **Customers** table and the **Orders** table. Write a SQL query to find all customers who never order anything.

Table: **Customers**.

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

Table: **Orders**.

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

Using the above tables as example, return the following:

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

> 解法

``` sql
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```


## 197. Rising Temperature

Given a **Weather** table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.

```
+---------+------------+------------------+
| Id(INT) | Date(DATE) | Temperature(INT) |
+---------+------------+------------------+
|       1 | 2015-01-01 |               10 |
|       2 | 2015-01-02 |               25 |
|       3 | 2015-01-03 |               20 |
|       4 | 2015-01-04 |               30 |
+---------+------------+------------------+
```
For example, return the following Ids for the above Weather table:

```
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```

> 解法

``` sql
select aa.id from (
select a.id , a.date,b.id as 'bid',b.date as 'bdate' from weather a join weather b on datediff(a.date,b.date)=1 and a.Temperature>b.Temperature ) aa;
```

## 596. Classes More Than 5 Students

There is a table **courses** with columns: student and class

Please list out all classes which have more than or equal to 5 students.

For example, the table:

```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```
Should output:

```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

**Note:**
The students should not be counted duplicate in each course.


> 解法


``` sql
select class from courses group by class having count(DISTINCT student) >= 5;
```


## 196. Delete Duplicate Emails

Write a SQL query to delete all duplicate email entries in a table named **Person**, keeping only unique emails based on its smallest Id.

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
```
Id is the primary key column for this table.
For example, after running your query, the above Person table should have the following rows:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

> 解法

``` sql
delete from person where id not in 
	(select minid from 
		(select min(id) as minid from person group by email) b)
```

## 176. Second Highest Salary

Write a SQL query to get the second highest salary from the **Employee** table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```
> 解法

``` sql
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary
```

## 608. Tree Node

Given a table **tree**, id is identifier of the tree node and p_id is its parent node's id.

```
+----+------+
| id | p_id |
+----+------+
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |
+----+------+
```

Each node in the tree can be one of three types:
Leaf: if the node is a leaf node.
Root: if the node is the root of the tree.
Inner: If the node is neither a leaf node nor a root node.
Write a query to print the node id and the type of the node. Sort your output by the node id. The result for the above sample is:

```
+----+------+
| id | Type |
+----+------+
| 1  | Root |
| 2  | Inner|
| 3  | Leaf |
| 4  | Leaf |
| 5  | Leaf |
+----+------+
```

Explanation

- Node '1' is root node, because its parent node is NULL and it has child node '2' and '3'.
- Node '2' is inner node, because it has parent node '1' and child node '4' and '5'.
- Node '3', '4' and '5' is Leaf node, because they have parent node and they don't have child node.

And here is the image of the sample tree as below:

```
			  1
			/   \
	              2       3
		    /   \
	         4       5
```
Note

If there is only one node on the tree, you only need to output its root attributes.

> 解法

``` sql
select T.id,
(case 
when T.p_id is null then 'Root'
when T.id in (select p_id from tree) then 'Inner'
else 'Leaf' end) as Type
from tree T;
```

## 570. Managers with at Least 5 Direct Reports

The **Employee** table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

```
+------+----------+-----------+----------+
|Id    |Name 	  |Department |ManagerId |
+------+----------+-----------+----------+
|101   |John 	  |A 	      |null      |
|102   |Dan 	  |A 	      |101       |
|103   |James 	  |A 	      |101       |
|104   |Amy 	  |A 	      |101       |
|105   |Anne 	  |A 	      |101       |
|106   |Ron 	  |B 	      |101       |
+------+----------+-----------+----------+
```

Given the **Employee** table, write a SQL query that finds out managers with at least 5 direct report. For the above table, your SQL query 

should return:

```
+-------+
| Name  |
+-------+
| John  |
+-------+
```
**Note**:
No one would report to himself.

> 解法

``` sql
SELECT e1.Name 
FROM Employee AS e1 INNER JOIN Employee AS e2 
ON e1.Id = e2.ManagerId 
GROUP BY e1.Name 
HAVING Count(e1.Name) >= 5
```


## 602. Friend Requests II: Who Has the Most Friends

In social network like Facebook or Twitter, people send friend requests and accept others' requests as well.

Table **request\_accepted** holds the data of friend acceptance, while **requester\_id** and **accepter\_id** both are the id of a person.

| requester_id | accepter_id | accept_date|
|--------------|-------------|------------|
| 1            | 2           | 2016_06-03 |
| 1            | 3           | 2016-06-08 |
| 2            | 3           | 2016-06-08 |
| 3            | 4           | 2016-06-09 |

Write a query to find the the people who has most friends and the most friends number. For the sample data above, the result is:

| id | num |
|----|-----|
| 3  | 3   |

**Note:**

- It is guaranteed there is only 1 people having the most friends.
- The friend request could only been accepted once, which mean there is no multiple records with the same **requester_id** and **accepter_id** value.

**Explanation:**

The person with id '3' is a friend of people '1', '2' and '4', so he has 3 friends in total, which is the most number than any others.

**Follow-up:**

In the real world, multiple people could have the same most number of friends, can you find all these people in this case?

> 解法

``` sql
select ids as id, cnt as num from (
select ids,count(*) as cnt from (
select r1.requester_id as ids from request_accepted r1
Union all select r2.accepter_id from request_accepted r2) temp group by ids) as tbl
order by cnt desc limit 1
```

## 585. Investments in 2016

Write a query to print the sum of all total investment values in 2016 (**TIV_2016**), to a scale of 2 decimal places, for all policy holders who meet the following criteria:

1. Have the same **TIV_2015** value as one or more other policyholders.
2. Are not located in the same city as any other policyholder (i.e.: the (latitude, longitude) attribute pairs must be unique).
Input Format:
The insurance table is described as follows:

| Column Name | Type          |
|-------------|---------------|
| PID         | INTEGER(11)   |
| TIV_2015    | NUMERIC(15,2) |
| TIV_2016    | NUMERIC(15,2) |
| LAT         | NUMERIC(5,2)  |
| LON         | NUMERIC(5,2)  |
where PID is the policyholder's policy ID, **TIV_2015** is the total investment value in 2015, **TIV\_2016** is the total investment value in 2016, LAT is the latitude of the policy holder's city, and LON is the longitude of the policy holder's city.

Sample Input

| PID | TIV_2015 | TIV_2016 | LAT | LON |
|-----|----------|----------|-----|-----|
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |
Sample Output

| TIV_2016 |
|----------|
| 45.00    |

**Explanation**

The first record in the table, like the last record, meets both of the two criteria.
The TIV_2015 value '10' is as the same as the third and forth record, and its location unique.

The second record does not meet any of the two criteria. Its TIV_2015 is not like any other policyholders.

And its location is the same with the third record, which makes the third record fail, too.

So, the result is the sum of TIV_2016 of the first and last record, which is 45.

> 解法

``` sql
SELECT
    SUM(insurance.TIV_2016) AS TIV_2016
FROM
    insurance
WHERE
    insurance.TIV_2015 IN
    (
      SELECT
        TIV_2015
      FROM
        insurance
      GROUP BY TIV_2015
      HAVING COUNT(*) > 1
    )
    AND CONCAT(LAT, LON) IN
    (
      SELECT
        CONCAT(LAT, LON)
      FROM
        insurance
      GROUP BY LAT , LON
      HAVING COUNT(*) = 1
    )

```


## 626. Exchange Seats

Mary is a teacher in a middle school and she has a table **seat** storing students' names and their corresponding seat ids.

The column id is continuous increment.
Mary wants to change seats for the adjacent students.
Can you write a SQL query to output the result for Mary?

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```
For the sample input, the output is:

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```
**Note:**

If the number of students is odd, there is no need to change the last one's seat.

> 解法

``` sql
select 
  ( case when id%2!=0 and cnt!=id then id+1  
        when id%2=0 and cnt!=id  then id-1
        when id %2 = 0 and cnt = id then id -1
        else id end ) as id, student
from seat s1 join (select count(*) AS cnt from seat ) as temp 
order by id  ;
```

## 580. Count Student Number in Departments

A university uses 2 data tables, **student** and **department**, to store data about its students and the departments associated with each major.

Write a query to print the respective department name and number of students majoring in each department for all departments in the **department** table (even ones with no current students).

Sort your results by descending number of students; if two or more departments have the same number of students, then sort those departments alphabetically by department name.

The **student** is described as follow:

| Column Name  | Type      |
|--------------|-----------|
| student_id   | Integer   |
| student_name | String    |
| gender       | Character |
| dept_id      | Integer   |

where student\_id is the student's ID number, student\_name is the student's name, gender is their gender, and dept\_id is the department ID associated with their declared major.

And the department table is described as below:

| Column Name | Type    |
|-------------|---------|
| dept_id     | Integer |
| dept_name   | String  |

where dept\_id is the department's ID number and dept\_name is the **department** name.

Here is an example input:
**student** table:

| student_id | student_name | gender | dept_id |
|------------|--------------|--------|---------|
| 1          | Jack         | M      | 1       |
| 2          | Jane         | F      | 1       |
| 3          | Mark         | M      | 2       |

**department** table:

| dept_id | dept_name   |
|---------|-------------|
| 1       | Engineering |
| 2       | Science     |
| 3       | Law         |

The **Output** should be:

| dept_name   | student_number |
|-------------|----------------|
| Engineering | 2              |
| Science     | 1              |
| Law         | 0              |

> 解法

``` sql
select d.dept_name, count(student_id) as 'student_number'
from department d
left join student s on s.dept_id=d.dept_id 
group by d.dept_name
order by student_number DESC,d.dept_name;
```

## 574. Winning Candidate

Table: **Candidate**

```
+-----+---------+
| id  | Name    |
+-----+---------+
| 1   | A       |
| 2   | B       |
| 3   | C       |
| 4   | D       |
| 5   | E       |
+-----+---------+  
```
Table: **Vote**

```
+-----+--------------+
| id  | CandidateId  |
+-----+--------------+
| 1   |     2        |
| 2   |     4        |
| 3   |     3        |
| 4   |     2        |
| 5   |     5        |
+-----+--------------+
id is the auto-increment primary key,
CandidateId is the id appeared in Candidate table.
```

Write a sql to find the name of the winning candidate, the above example will return the winner **B**.

```
+------+
| Name |
+------+
| B    |
+------+
```
**Notes:**

You may assume there is **no tie**, in other words there will be **at most one** winning candidate.

> 解法


```sql
select c.name as 'Name' from Candidate c join
(select v.candidateid from Vote v group by v.candidateid order by count(v.candidateid) desc limit 1) as winner where c.id = winner.Candidateid ;
```


## 178. Rank Scores

Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

For example, given the above **Scores** table, your query should generate the following report (order by highest score):

```
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

> 解法

``` sql
SELECT
  Score,
  (SELECT count(distinct Score) FROM Scores WHERE Score >= s.Score) Rank
FROM Scores s
ORDER BY Score desc
```

## 180. Consecutive Numbers

Write a SQL query to find all numbers that appear at least three times consecutively.

```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

For example, given the above **Logs** table, 1 is the only number that appears consecutively for at least three times.

```
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

> 解法

``` sql
select distinct l1.num as ConsecutiveNums from 
logs l1, logs l2, logs l3
where l1.id+1=l2.id and l2.id+1=l3.id
and l1.num=l2.num and l2.num=l3.num;
```

## 614. Second Degree Follower

In facebook, there is a **follow** table with two columns: followee, follower.

Please write a sql query to get the amount of each follower’s follower if he/she has one.

For example:

```
+-------------+------------+
| followee    | follower   |
+-------------+------------+
|     A       |     B      |
|     B       |     C      |
|     B       |     D      |
|     D       |     E      |
+-------------+------------+
```

should output:

```
+-------------+------------+
| follower    | num        |
+-------------+------------+
|     B       |  2         |
|     D       |  1         |
+-------------+------------+
```

**Explaination:**

Both B and D exist in the follower list, when as a followee, B's follower is C and D, and D's follower is E. A does not exist in follower list.

**Note:**

Followee would not follow himself/herself in all cases.
Please display the result in follower's alphabet order.

> 解法

``` sql
Select f1.follower, count(distinct f2.follower) as num
from follow f1
inner join follow f2 on f1.follower = f2.followee
Group by f1.follower
```

> 错误的解法

``` sql
select distinct f1.followee as follower, count(distinct f1.follower)as num 
from follow f1  where 
f1.followee  in (select distinct f2.follower from follow f2 ) 
group by f1.followee ;
```

## 184. Department Highest Salary

The **Employee** table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

The **Department** table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, Max has the highest salary in the IT department and Henry has the highest salary in the Sales **department**.

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

> 解法

``` sql
SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM
    Employee
        JOIN
    Department ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
    )
;

```

## 177. Nth Highest Salary

Write a SQL query to get the nth highest salary from the Employee table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

> 解法

``` sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
# create function 创建一个方法
# returns int 表示返回值是 int类型 即数值而类型
# DECLARE M INT 定义M为Int类型的值
# SET M = N - 1;
# 表示 设置m的值
BEGIN
DECLARE M INT;
SET M=N-1;
  RETURN (
      # Write your MySQL query statement below.
      select distinct salary from Employee order by salary desc limit 1 offset M
  );
END
```

## 601. Human Traffic of Stadium

X city built a new stadium, each day many people visit it and the stats are saved as these columns: id, date, people

Please write a query to display the records which have 3 or more consecutive rows and the amount of people more than 100(inclusive).

For example, the table **stadium**:

```
+------+------------+-----------+
| id   | date       | people    |
+------+------------+-----------+
| 1    | 2017-01-01 | 10        |
| 2    | 2017-01-02 | 109       |
| 3    | 2017-01-03 | 150       |
| 4    | 2017-01-04 | 99        |
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-08 | 188       |
+------+------------+-----------+
```

For the sample data above, the output is:

```
+------+------------+-----------+
| id   | date       | people    |
+------+------------+-----------+
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-08 | 188       |
+------+------------+-----------+
```

**Note:**


Each day only have one row record, and the dates are increasing with id increasing.

> 解法

``` sql
select distinct s1.* from stadium s1, stadium s2, stadium s3 where s1.people >= 100 and s2.people >= 100 and s3.people >= 100 and 
(
    (s2.id - s3.id = 1 and s1.id - s2.id = 1 ) #1 2 3
     or (s2.id-s1.id=1 and s1.id-s3.id=1  ) # 2 1 3 
    or (s3.id-s2.id=1 and s2.id-s1.id=1 ) # 3 2 1
) order by s1.id;
```


## 615. Average Salary: Departments VS Company

Given two tables as below, write a query to display the comparison result (higher/lower/same) of the average salary of employees in a department to the company's average salary.


Table: **salary**

| id | employee_id | amount | pay_date   |
|----|-------------|--------|------------|
| 1  | 1           | 9000   | 2017-03-31 |
| 2  | 2           | 6000   | 2017-03-31 |
| 3  | 3           | 10000  | 2017-03-31 |
| 4  | 1           | 7000   | 2017-02-28 |
| 5  | 2           | 6000   | 2017-02-28 |
| 6  | 3           | 8000   | 2017-02-28 |


The employee\_id column refers to the employee\_id in the following table **employee**.


| employee_id | department_id |
|-------------|---------------|
| 1           | 1             |
| 2           | 2             |
| 3           | 2             |


So for the sample data above, the result is:


| pay_month | department_id | comparison  |
|-----------|---------------|-------------|
| 2017-03   | 1             | higher      |
| 2017-03   | 2             | lower       |
| 2017-02   | 1             | same        |
| 2017-02   | 2             | same        |


**Explanation**

In March, the company's average salary is (9000+6000+10000)/3 = 8333.33...

The average salary for department '1' is 9000, which is the salary of employee\_id '1' since there is only one employee in this department. So the comparison result is 'higher' since 9000 > 8333.33 obviously.

The average salary of department '2' is (6000 + 10000)/2 = 8000, which is the average of employee_id '2' and '3'. So the comparison result is 'lower' since 8000 < 8333.33.

With he same formula for the average salary comparison in February, the result is 'same' since both the department '1' and '2' have the same average salary with the company, which is 7000.

> 解法

``` sql
select temp1.pay_month, 
temp1.department_id,
(case when  temp1.mondepagvsalary>temp2.monthavgsalary then "higher"     
when  temp1.mondepagvsalary<temp2.monthavgsalary then "lower"     
else  "same" end)  as comparison
from 
(select distinct left(s.pay_date,7) as pay_month,
e.department_id,
sum(s.amount)/count(s.amount) as mondepagvsalary
from salary s
join employee e on s.employee_id=e.employee_id 
group by left(s.pay_date,7), e.department_id) as temp1
join
(select distinct left(s.pay_date,7) as pay_month,
sum(s.amount)/count(s.amount) as monthavgsalary 
from salary s  
join  employee e 
on s.employee_id=e.employee_id 
group by left(s.pay_date,7)) as temp2
on temp1.pay_month=temp2.pay_month;
```


## 185. Department Top Three Salaries

The **Employee** table holds all employees. Every employee has an Id, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
```

The **Department** table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, your SQL query should return the following rows.

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

> 解法

``` sql
SELECT
    d.Name AS 'Department', e1.Name AS 'Employee', e1.Salary
FROM
    Employee e1
        JOIN
    Department d ON e1.DepartmentId = d.Id
WHERE
    3 > (SELECT
            COUNT(DISTINCT e2.Salary)
        FROM
            Employee e2
        WHERE
            e2.Salary > e1.Salary
                AND e1.DepartmentId = e2.DepartmentId
        )
;
```
> 另一种解法

``` sql
select d.Name as 'Department', e1.Name as 'Employee', e1.Salary 
from Employee e1 join Department d on e1.DepartmentId = d.Id
where 
(select count(distinct e2.Salary) 
 from Employee e2 WHERE e2.Salary > e1.Salary AND e1.DepartmentId = e2.DepartmentId) 
 < 3;
```


## 262. Trips and Users

The **Trips** table holds all taxi trips. Each trip has a unique Id, while Client_Id and Driver_Id are both foreign keys to the Users_Id at the **Users** table. Status is an ENUM type of (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’).

```
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
```

The **Users** table holds all users. Each user has an unique Users_Id, and Role is an ENUM type of (‘client’, ‘driver’, ‘partner’).

```
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
```

Write a SQL query to find the cancellation rate of requests made by unbanned clients between Oct 1, 2013 and Oct 3, 2013. For the above tables, your SQL query should return the following rows with the cancellation rate being rounded to two decimal places.

```
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+
```

> 解法

``` sql
select 
t.Request_at Day, 
round(sum(case when t.Status like 'cancelled_%' then 1 else 0 end)/count(*),2) as 'Cancellation Rate'
from Trips t 
inner join Users u 
on t.Client_Id = u.Users_Id and u.Banned='No'
where t.Request_at between '2013-10-01' and '2013-10-03'
group by t.Request_at
```

这道题目其实不难 看的懂like匹配就好了
