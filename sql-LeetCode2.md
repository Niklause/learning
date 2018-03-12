
## 第一题

Given a table tree, id is identifier of the tree node and p_id is its parent node's id.

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

Node '1' is root node, because its parent node is NULL and it has child node '2' and '3'.
Node '2' is inner node, because it has parent node '1' and child node '4' and '5'.
Node '3', '4' and '5' is Leaf node, because they have parent node and they don't have child node.

And here is the image of the sample tree as below:


	          1
	        /   \
           2     3
        /   \
      4       5

正确的解法：

``` sql
select T.id, 
IF(isnull(T.p_id), 'Root', IF(T.id in (select p_id from tree), 'Inner', 'Leaf')) Type 
from tree T
```

你的答案：

``` sql
Select  a  as id ,  
(case when b is null and c is null then “Root”
When b is not null and c is not null then “Leaf”
Else “Inner”) as type
From 
(Select t1.id as a, t2.id as b , t3.id as c From tree t1 
left join tree t2 on t1.p_id=t2.id
left join tree t3 on t2.p_id=t3.id ) temp;
```

解析：

这道题目的主要问题应该是没有理解题意，树属于数据结构中的一种特殊的数据结构。这个题目的题意应该是这样去理解 p_id就是parent id的意思。那么如果这个pid是null的，则这个点就是一个Root点，如果一个点在parent id中有这个点那么这个点就是一个inner的点 否则的话就是 一个叶子点 即leaf

理解这些 后面的就好做了

IF的用法 就和python中的差不多 就是这样去用就好了 


## 第二题

Table number contains many numbers in column num including duplicated ones.
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
Note:
If there is no such number, just output null.

正确答案：

``` sql
select ifnull( (select num from ( select num  
from number
group by num
having count(num) =  1
order by num desc 
limit 1
) as maxnum)  , null) as num
```

你的答案：

``` sql
Select distinct num from number group by num order by count(num) desc limit 1;
```

解析：

这个题目你这个答案是属于直接询问这个 表里最大的值，但是呢答案是问只出现一次的最大的值，所以应该先把只出现一次的值提取出来，然后再在里面进行排序。

因为容易出现空值所以就应该对空值进行设定

ifnull(a,b) 这个函数的意思是如果a是null值那么就返回b值 b值可以为任何值

## 第三题：

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

Note: The longest distance among all the points are less than 10000.

``` sql
select ROUND(SQRT(min((p1.x-p2.x)*(p1.x-p2.x)+(p1.y-p2.y)*(p1.y-p2.y))),2) as shortest
from point_2d p1,point_2d p2
where p1.x != p2.x or p1.y != p2.y;
```

这个题目没难度  认真一点就好了

## 第四题

In social network like Facebook or Twitter, people send friend requests and accept others' requests as well.

Table request_accepted holds the data of friend acceptance, while requester_id and accepter_id both are the id of a person.

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

Note:

It is guaranteed there is only 1 people having the most friends.
The friend request could only been accepted once, which mean there is no multiple records with the same requester_id and accepter_id value.

Explanation:

The person with id '3' is a friend of people '1', '2' and '4', so he has 3 friends in total, which is the most number than any others.

Follow-up:

In the real world, multiple people could have the same most number of friends, can you find all these people in this case?

你的答案：

```
Select temp.requester_id as id , count(temp.accepter_id ) as num from 
(select r1.* from request_accepted r1
Union select r2.accepter_id, r2.requester_id from request_accepted r2) temp
Group by temp.requester_id order by count(temp.accepter_id ) desc limit 1;
```



题目比较简单 表中有两个数据 requester\_id 和accepter\_id 这个表示 发起 添加好友请求和 接受好友添加请求，这个数据肯定都是唯一的，所以我们要计算每个人好友的数量只需要去查询在这个表中id出现的数目就好了。

这个答案出错的原因有好几个

比较主要的原因是

union连接的两个表 所查询的数据列数必须一直，否则就没有办法把数据合并起来了

```select r1.* from request_accepted r1``` 和 ```select r2.accepter_id, r2.requester_id from request_accepted r2```的列数不一致，第一个是三列，第二个是两列所以这个地方会有错误

然后其实查询出的数据都是一个id，那么我们就统一命名成ids就好了。

然后去计算ids出现的数量拿出最大的就好了

这里面的难点主要是union的用法:

union内部的select语句必须拥有**相同数量的列**，列也必须拥有相似的数据类型。

同样，每条SELECT语句中的**列的顺序**必须相同！！！

union用于合并两个或多个select预取的结果集，并消除表中任何**重复行**

如果允许**重复的值**，请使用**UNION ALL**

正确答案

``` sql
select ids as id, cnt as num from (
select ids,count(*) as cnt from (
select r1.requester_id as ids from request_accepted r1
Union all select r2.accepter_id from request_accepted r2) temp group by ids) as tbl
order by cnt desc limit 1
```

这样也可以

```sql
select ids, count(*) as cnt
    from
    (
         select requester_id as ids from request_accepted
         union all
         select accepter_id from request_accepted
     ) as tbl1
    group by ids
    ) as tbl2
 order by cnt desc
 limit 1
;
```



## 第五题：

In social network like Facebook or Twitter, people send friend requests and accept others’ requests as well. Now given two tables as below:

Table: ```friend_request```


| sender_id | send_to_id |request_date|
|-----------|------------|------------|
| 1         | 2          | 2016_06-01 |
| 1         | 3          | 2016_06-01 |
| 1         | 4          | 2016_06-01 |
| 2         | 3          | 2016_06-02 |
| 3         | 4          | 2016-06-09 |

Table: ```request_accepted```

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

Note:
The accepted requests are not necessarily from the table friend_request. In this case, you just need to simply count the total accepted requests (no matter whether they are in the original requests), and divide it by the number of requests to get the acceptance rate.

It is possible that a sender sends multiple requests to the same receiver, and a request could be accepted more than once. In this case, the ‘duplicated’ requests or acceptances are only counted once.

If there is no requests at all, you should return 0.00 as the accept_rate.

Explanation: There are 4 unique accepted requests, and there are 5 requests in total. So the rate is 0.80.

Follow-up:
Can you write a query to return the accept rate but for every month?
How about the cumulative accept rate for every day?

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

解析：

这个题意其实很简单。

两个表中有不同的request请求，第一个表示发起请求加为好友，第二个接受请求加为好友。

第二个表中有一些数据是相同的，所要做的就是 用第二个表中所有的不同的数据除以第一个表中不同观点数据，得到的结果就是请求成功率~

这个的主要难点是ifnull和round的用法

ifnull 前面讲过了

round(a,b) a为得到的值 b为小数点个数，如果不填就默认为 0.如果是2的话就是小数点后两位。


## 第六题

The Employee table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

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
Given the Employee table, write a SQL query that finds out managers with at least 5 direct report. For the above table, your SQL query should return:

```
+-------+
| Name  |
+-------+
| John  |
+-------+

```
Note:
No one would report to himself.


这道题嘛 其实蛮简单的。。。我觉得 你自己再仔细考虑下 是可以做对的

``` sql
Select e1.name
From employee e1 
Left join employee e2 on e1.managerid=e2.id
Group by e1.managerid having count(e1.id)>=5;
```

这道题目 不用 left join 也可以做，直接用jion其实就可以的

但是如果用leftjion的话一定要记住 你要计算的是e2中的值 不是e1中的值

left join是以 e1 为基础 然后对e1中的managerid 绑定e2中的id 这样的话只要根据 e2中name进行分类 并把e2中name数量大于5的输出就好了


下面给你一个 用inner join做的方法

``` sql
SELECT e1.Name 
FROM Employee AS e1 INNER JOIN Employee AS e2 
ON e1.Id = e2.ManagerId 
GROUP BY e1.Name 
HAVING Count(e1.Name) >= 5
```

