# 第N高的薪水

编写一个 SQL 查询，获取 Employee 表中第N高的薪水（Salary） 。

```mysql
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。==如果不存在第N高的薪水，那么查询应返回 null==。

```mysql
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

**我的思路1：**

- 将不同的薪资按降序排序，然后使用 `LIMIT`子句获得第二高的薪资
- 注意：如果表中只有一条记录，那么得返回null，所以需要将查询的结果作为临时表

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    declare m int;  #注意：limit里不能做运算，所以需要在这里定义一个变量
    set m = n-1;
  RETURN (   
      select (
          select distinct Salary from Employee
          order by Salary desc
          limit m,1  #第N大，偏移N-1个，选1个
      )
  );
END
```

# 分数排名

编写一个 SQL 查询来实现分数排名。

如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

```sql
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

例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

```mysql
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

重要提示：对于 MySQL 解决方案，如果要转义用作列名的保留字，可以在关键字之前和之后使用撇号。例如 \'Rank`

**我的思路：**

- 第一列的Score直接select
- 第二列的Rank，可以这样计算：将分数去重后数有多少个分数大于等于当前分数，得到的个数就是当前分数的排名

```mysql
select a.Score as Score,
(select count(distinct b.Score) from Scores b where b.Score >= a.Score) as `Rank`
from Scores a
order by a.Score desc
```

# 连续出现的数字

表：Logs

```go
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
```

id 是这个表的主键。


编写一个 SQL 查询，查找所有至少连续出现三次的数字。

返回的结果表中的数据可以按 任意顺序 排列。

查询结果格式如下面的例子所示： 

Logs 表：

```go
+----+-----+
| Id | Num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
```

Result 表：

```go
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

1 是唯一连续出现至少三次的数字。

**我的思路：**

- 表自身连接，连续3次出现的数字的特征：id1=id2-1, id2=id3-1 && num1=num2=num3

```mysql
select distinct l1.num as ConsecutiveNums
from Logs l1, Logs l2, Logs l3
where l1.id=l2.id-1 
and l2.id=l3.id-1
and l1.num=l2.num
and l2.num=l3.num
```

# 超过经理收入的员工

Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

```go
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

```go
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

**我的思路：**

- 表与自身通过经理进行连接，在新的大表中找薪水大于经理薪水的

```mysql
# 表与自身通过经理进行连接
select a.Name as Employee from Employee a 
inner join Employee b  #inner可以省略
on a.ManagerId=b.Id 
and a.Salary>b.Salary  #and也可以是where
```

# 查找重复的电子邮箱

编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。

示例：

```sql
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

根据以上输入，你的查询应返回以下结果：

```sql
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**我的思路：**

- 将电子邮箱按名字分组，找组里面数量>=2的

```mysql
# 将电子邮箱按名字分组，找组里面数量>=2的
select Email from Person
group by Email
having count(Email)>=2
```

# 从不订购的客户

某网站包含两个表，Customers 表和 Orders 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

Customers 表：

```go
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

Orders 表：

```go
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

例如给定上述表格，你的查询应返回：

```go
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

**我的思路：**

- 左连接之后，新表中查找CUstomerid为空的

```mysql
# 左连接，查找CUstomerid为空的
select a.Name as Customers
from Customers a
left join Orders b
on a.Id=b.CustomerId
where b.CustomerID is null
```

# 部门工资最高的员工

Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

Department 表包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资最高的员工。对于上述表，您的 SQL 查询应返回以下行（行的顺序无关紧要）。

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

**我的思路：**

- 先对两张表通过部门作内连接
- 再从大表里面筛选出部门工资最高的，筛选最高工资可以用max(Salary)

```mysql
# 先对两张表通过部门作内连接
select b.Name as Department,a.Name as Employee,a.Salary as Salary
from Employee a 
inner join Department b
on a.DepartmentId=b.Id  
# 再从大表里面筛选出工资最高的
where (a.DepartmentId,a.Salary) in (
    select DepartmentId,max(Salary)   #注意：聚集函数max只能用于select与having中，不能用于where中
    from Employee
    group by DepartmentId
)
```

# 部门工资前3高的所有员工

Employee 表包含所有员工信息，每个员工有其对应的工号 Id，姓名 Name，工资 Salary 和部门编号 DepartmentId 。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```

Department 表包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门获得前三高工资的所有员工。例如，根据上述给定的表，查询结果应返回：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

**我的思路：**

- 两张表先做内连接
- 再在大表里筛选出工资前3大的：前3大的，则工资比他大的小于3个

```mysql
# 两张表先做内连接
select b.Name as Department, a.Name as Employee, a.Salary as Salary
from Employee a 
inner join Department b
on a.DepartmentId=b.Id 

# 再在大表里筛选出工资前3大的：前3大的，则工资比他大的小于3个
where (
    select count(distinct c.Salary) 
    from Employee c
    where c.Salary > a.Salary and a.DepartmentId=c.DepartmentId
)  < 3;
```

# 删除重复的电子邮箱

编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
```

Id 是这个表的主键。
例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

**我的思路：**

- 自身作连接，然后删除p1.Email=p2.Email and p1.Id > p2.Id的

```mysql
delete p1 from Person p1
inner join Person p2
on p1.Email=p2.Email 
and p1.Id>p2.Id

#或者使用笛卡尔积
delete p1 from Person p1, Person p2
where p1.Email=p2.Email and p1.Id>p2.Id
```

