```sql

# 更新工资
update salary (set sex="m" where sex="f" ) or (set sex='f' where sex='m');

update salary 
case when sex='f' then sex='m' 
eles sex='m';

update salary 
set sex='f' case when  sex='m' 
eles sex='m';

# 正确答案
UPDATE salary SET sex = case sex WHEN 'm' THEN 'f' ELSE 'm' END;

/*
* update的用法 
*/

UPDATE table_name SET field1=new-value1, field2=new-value2

case when的用法有两种

#第一种
case result
	when '胜' then 1
	when '负' then 2
else 0
end

#第二种

case when result ='胜' then 1
when result='负' then 2
else 0
end

case when 大概就是这两种用法 不外乎 result 的位置，记住就好了~

or 可以在where子语句中把两个或多个条件组合起来 但是一般不如上面那么使用

# 第二个

select p.fistname,p.lastname,a.city,a.state from person p join address on p.personid=a.personid;

# 正确答案

select FirstName, LastName, City, State from Person right join Address on Person.PersonId = Address.PersonId;

首先 join的用法

join一共有五种用法 
join / inner join / left join / right join / full join

其中join和inner join的用法是一样的

# join:  如果两个表中有至少一个匹配，则返回行
# left join: 即使右表中没有匹配，也从左表中返回所有的行
# right join: 即使左表中没有匹配，也从右表中返回所有的行
# full join: 只要其中一个表存在匹配，就返回行

# 第三道

select a.name from (
    select a.name, a.salary ,b.name,b.salary from employee a
      left join employee b on a.managerid=b.id)
      where a.salary > b.salary

# 正确答案

SELECT a.name as 'Employee' FROM Employee as a, Employee as b
WHERE a.ManagerId = b.id and a.Salary > b.Salary;

# 经错误答案更改后的正确答案

select name from (
    select a.name, a.salary ,b.name as 'bname',b.salary as 'bsalary' from employee a
      left join employee b on a.managerid=b.id) aa
      where salary > bsalary

# 这道题有这么几个问题
# 首先当你从一个临时的表中选择数据的时候 需要在后面添加一个表的名字 如 aa
# 其次，无法从有重复名字的表中获取数据 所以 可以设置别名 如b.name as 'bname'
# 第三 如果只看括号中的数据 表头的名称就是 'name','salary','bname','bsalary'
# 里面的别名a,b在括号之外就无法使用了，这是一个作用域的问题


# 第四道

select name from customers where name not in (
select name from customers join order on customers.id= order.customerid );
# 正确答案
select name from customers left join orders on customers.id= orders.customerid where orders.id is null;

# 这道题目问题不大，主要拼写的问题
# 然后就是 要注意name可能发生的重名的问题。

# 第五道

select aa.id from (
select a.id , a.date,b.id,b.date from weather a join weather b on a.id=b.id where datediff(b.date,a.date)=1 and a.temperture>b.temperture ) aa;

# 正确答案

select weather.id as 'Id' from weather join weather w on DATEDIFF(weather.Date,w.Date) = 1 and weather.Temperature > w.Temperature;

修改后的正确答案

select aa.id from (
select a.id , a.date,b.id as 'bid',b.date as 'bdate' from weather a join weather b on datediff(a.date,b.date)=1 and a.Temperature>b.Temperature ) aa;

# 注意事项:还是 要注意别名的问题
# 另外如果不需要join的数据就不用显示的join
# DATEDIFF(a,b) mysql和sql server中的用法是不一样的，在mysql中是 a-b 在sql server中是 b-a这种微小的差别在使用过程中要注意。

# 第六道

select class from courses group by class having count(student) >=5;

正确答案:
select class from courses group by class having count(DISTINCT student) >= 5;

去除重复数据就好了


# 第七道

delete from person where id not in (
select min(id) from person group by email);

delete from person where id not in (
select distinct email , min(id) from person ) ;

delete  * from person where id not in (
select distinct email , min(id) from person  group by email) ;

# 正确答案：
DELETE p1 FROM Person p1, Person p2 WHERE p1.Email = p2.Email AND p1.Id > p2.Id

# 首先delete的用法
DELETE FROM 表名称 WHERE 列名称 = 值

当from后面有两个表的时候 需要指定删除的表的名称

# 修改后的正确答案
delete from person where id not in (select minid from (select min(id) as minid from person group by email) b)

# 其实一开始的思路是对的，但是有一个问题
# 更新数据时使用了查询，而查询的数据又做了更新的条件，mysql不支持这种操作方式。
# 所以就需要做一层封装，先查询出结果之后再查询一次结果，这样 mysql就不会认为在进
# 行查询操作，就可以进行更新操作了（所以说 这里面都是坑啊哈哈哈哈哈哈

```
