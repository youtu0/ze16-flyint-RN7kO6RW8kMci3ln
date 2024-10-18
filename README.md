
# 函数



> 在Mysql中函数是一组预定义的指令,用于执行特定的操作并返回结果,可类比Java中的方法.在SQL中函数根据其**作用范围和返回结果**方法分为两大类:单行函数,分组函数


# 单行函数



> 单行函数的特点为对一行数据进行操作,并只返回一种结果.单行函数通常用于处理单个记录数据


* 单行函数又可分为:字符函数,数学函数,其他函数,流程控制函数


## 字符函数


* `CHAR_LENGTH(S),LENGTH(S):`返回字符串的长度


eg:查询员工姓名，姓名字数。



```
SELECT emplyee_name,CHARACTER_LENGTH(emplyee_name) FROM emplyees;

```

* `CONCAT(S1,S2,…Sn):`将两个以上的字符串连接


eg:将字符串'aaa','bbb','ccc'进行拼接。



```
SELECT CONCAT('aaa','bbb','ccc');

```

* `UPPER(),LOWER():`对字符进行大小写转化


eg:：查询员工邮箱，并转为大写显示



```
SELECT UPPER(email) FROM emplyees;

```

* `substr,substring(S, start, length):`提取字符串S从start位置开始,长度为length的字字符串


eg:提取hello world中的hello



```
SELECT substr('hello world',1,5);`

```

* `replace(S, old, new):` 在字符串S中将所有的old替换为new


eg:查询员工电话号码，要求去除中间的横线 ’\-’



```
SELECT REPLACE(phone_number, '-', '') FROM emplyees;

```

## 数学函数


* `ROUND(X):`对浮点数X进行四舍五入


eg:查询员工工资，和其四舍五入的整数值



```
SELECT salary,ROUND(salary) FROM employees;

```
* `CEIL(X):`对浮点数X向上取整,即返回≥X的最小整数


eg:查询员工工资，并且向上取整



```
SELECT salary,CEIL(salary) FROM employees;

```
* `FLOOR(X):`对浮点数X向下取整,即返回≤X的最大整数


eg:查询员工工资，并且向下取整



```
SELECT salary,FLOOR(salary) FROM employees;

```
* `TRUNCATE(X,length):`对浮点数的小数部分进行截取→常用于进行保留小数操作



```
SELECT TRUNCATE(1.9999,2);->1.99

```
* `MOD(X,Y)`:对两个数进行区域操作即X%Y


## 日期函数


* `NOW(),SYSDATE()`:返回当前系统日期\+时间
* `CURDATE():`返回当前系统日期,不包括时间
* `CURTIME()`:返回当前系统时间,不包括日期
* `DATE_FORMAT(date,format):` 用于格式化日期,date是要格式化的数据,format是格式化的模式,格式化的通配符号如下表:




| **格式符** | **功能** |
| --- | --- |
| %Y | 4位年份 |
| %y | 2位年份 |
| %m | 月份（01，02,…,11,12） |
| %c | 月份（1,2,…,11,12） |
| %d | 日（01，02，…） |
| %H | 小时（24小时制） |
| %h | 小时（12小时制） |
| %i | 分钟（00，01，…,58,59） |
| %s | 秒（00，01，…,58,59） |


eg:查询员工姓名、入职时间，入职时间按照xxxx年xx月xx日输出



```
SELECT DATE_FORMAT(hiredate,'%Y年%m月%d日') FROM employees;

```

## 流程控制函数



> 流程控制函数在SQL中根据条件选择性地返回不同的结果,其允许在查询过程中实现条件逻辑


* `IF(expr,true_val,false_val):` 若表达式`expr`为真,则返回结果`true_val`,否则返回`false_val`的结果


eg: 如果查询的年纪大于18则返回adult,否则返回minor



```
SELECT age,IF(age>=18,'adult','minor');

```

* `CASE`语法结构:类似于switch…case结构,用于实现多支路条件选择



```
SELECT exper1,exper2...,
CASE exper1
		 WHEN value1 THEN result1
		 WHEN value2 THEN result2
		 WHEN value3 THEN result3
		 ...
		 ELSE result
END
FROM table_name;

```

eg:根据查询的部门号返回部门名称



```
SELECT department_id
CASE department_id
		 WHEN 1 THEN '经理办公室'
		 WHEN 2 THEN '财务部'
		 WHEN 3 THEN '后勤部'
		 ELSE 'unkown'
END AS department_name
FROM departments;

```

* 搜索CASE:语法结构与朴素CASE类似,但搜索CASE可根据多种不同的表达式条件返回不同值



```
SELECT exper1,exper2...,
CASE 
		 WHEN condition 1 THEN result1
		 WHEN condition 2 THEN result2
		 ...
		 ELSE result
END 
FROM table_name;

```

eg:查询员工姓名以及工资，工资按照一定规则发放，入职时间在2015\-01\-01之前的员工工资\*2，入职时间在2018\-01\-01之前的员工工资\*1\.5，其他不变



```
SELECT 
employee_name,hiredate,salary 原工资,
CASE
    WHEN hiredate<'2015-01-01' THEN salary*2
		WHEN hiredate<'2018-01-01' THEN salary*1.3
		ELSE salary
END AS 新工资
FROM employees;

```

# 分组函数



> 分组函数也称为聚合函数,用于对一组值进行操作,并返回单个结构


* `COUNT(*):`计算指定列中非NULL值的数量,`SUM(column)`:计算指定列之和,`AVG(column)`:计算指定列平均数,`MAX(colum):`取出指定列最大值,`MIN(colum)`:取出指定列最小值


eg:查询所有员工工资总和、平均值、最大值、最小值、员工个数;



```
SELECT SUM(salary),AVG(salary),MAX(salary),MIN(salary),COUNT(*) FROM employees;

```

# 分组查询



> 分组查询可根据某个或某些列对数据进行分组


## 按单个字段分组


* 通过`GROUP BY`关键字:按指定列进行分组



```
SELECT 列表
FROM 表
[WHERE 筛选条件]
GROUP BY 分组
[ORDER BY 排序]

```

eg:查询每个部门的最高工资



```
SELECT MAX(salary) 
FROM employees 
GROUP BY department_id;

```

## 在分组前进行条件筛选



> 在GROUP BY语句之间使用 WHERE语句对查询结果降序筛选


eg:查询每个部门入职时间在2010\-01\-01之后，并且工资最高的员工信息



```
SELECT *
FROM employees
WHERE hiredate >'2010-01-01'
GROUP BY department_id;

```

## 在分组之后进行条件筛选


* 通过`HAVING`语句可以在`GRUOP BY`语句之后进行条件筛选


eg:查询员工人数大于120的部门



```
SELECT *
FROM employees
GROUP BY department_id
HAVING COUNT(*)>120;

```


## 按多字段分组


* 在GRUOP BY语句后可接多个字段实现多字段分组


eg:查询每个部门，男女员工的平均工资



```
SELECT department_id,sex,AVG(salary) AS 平均工资
FROM employees
GROUP BY department_id,sex;

```

# 连接查询



> 连接查询是SQL中十分重要的知识点,就有”连接不会,通宵也白搭”,连接查询也称为多表查询,用于将**两个以上的表的数据基于某些相关条件组合在一起**,通过连接查询,可以从表中提取数据,生成一个新的结果集


* **笛卡尔积现象:**假设有两个表,表1有n行数据,表2有m行数据,**在使用连接查询时会产生n\*m条数据,因此在条件查询时需要假设连接的条件**


## 内连接(INNER JOIN)



> 内连接用于返回**两个表中所有满足连接条件的所有行数据,内连接可分为:等值连接,非等值连接,自连接**


### 等值连接



> 等值连接是一种常见的连接方式,其基于两表中某一列的相等条件进行连接


其语法结构如下:



```
SELECT colum1,colum2,....,
FROM table1 
INNER JOIN table2
ON table1.colum = table.colum;

```

eg:查询员工姓名以及所在的部门名称



```
SELECT employee_name AS 员工名,department_name AS 部门名
FROM employees e	
INNER JOIN departments d ON 
e.department_id=d.department_id;

```

### 非等值连接



> 非等值连接基于两表中某一列的不等条件进行连接.如大于,小于,不等于等等


**语法结构:**



```
SELECT colum1,colum2,....,
FROM table1 
INNER JOIN
ON table1.colum <operator> table2.colum;

```

其中`operator`可以是`>,<,≥,≤,≠,BETWEEN…AND`等等;


eg:查询员工工资及工资等级



```
SELECT e.salary,j.grade_level
FROM employees e
INNER JOIN job_grades j
ON e.salary BETWEEN j.lowest_sal AND j.higest_sal;

```

### 自连接



> 自连接是指同一个表的连接.这种连接通常用于处理表中有层次结构或函数递归关系的数据


eg:查询员工姓名以及对应的直系领导



```
SELECT t1.employee_name AS 员工,t2.employee_name AS 领导
FROM employees t1
INNER JOIN employees t2
ON t1.manager_id=t2.employee_id;

```

## 外连接



> 外连接用于返回主表中满足连接条件的行,同时保留另一个表中没有匹配的行


### 左/右外连接


* 左(右)连接顾名思义即根据表位置区分主表,即在左连接即左表,右连接即右侧


**语法结构:**



```
SELECT colum1,colum2...,
FROM table1 [LEFT|RIGHT]
JOIN ON [连接条件];

```

eg:查询员工姓名以及所在的部门名称，没有部门信息的员工也要查询出来



```
SELECT employee_name AS 员工姓名,department_name AS 部门名称
FROM employees e LEFT
JOIN departments d
ON e.department_id=d.department_id;

```

  * [函数](#%E5%87%BD%E6%95%B0)
* [单行函数](#%E5%8D%95%E8%A1%8C%E5%87%BD%E6%95%B0)
* [字符函数](#%E5%AD%97%E7%AC%A6%E5%87%BD%E6%95%B0)
* [数学函数](#%E6%95%B0%E5%AD%A6%E5%87%BD%E6%95%B0)
* [日期函数](#%E6%97%A5%E6%9C%9F%E5%87%BD%E6%95%B0)
* [流程控制函数](#%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%87%BD%E6%95%B0)
* [分组函数](#%E5%88%86%E7%BB%84%E5%87%BD%E6%95%B0)
* [分组查询](#%E5%88%86%E7%BB%84%E6%9F%A5%E8%AF%A2)
* [按单个字段分组](#%E6%8C%89%E5%8D%95%E4%B8%AA%E5%AD%97%E6%AE%B5%E5%88%86%E7%BB%84)
* [在分组前进行条件筛选](#%E5%9C%A8%E5%88%86%E7%BB%84%E5%89%8D%E8%BF%9B%E8%A1%8C%E6%9D%A1%E4%BB%B6%E7%AD%9B%E9%80%89)
* [在分组之后进行条件筛选](#%E5%9C%A8%E5%88%86%E7%BB%84%E4%B9%8B%E5%90%8E%E8%BF%9B%E8%A1%8C%E6%9D%A1%E4%BB%B6%E7%AD%9B%E9%80%89)
* [按多字段分组](#%E6%8C%89%E5%A4%9A%E5%AD%97%E6%AE%B5%E5%88%86%E7%BB%84)
* [连接查询](#%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A2):[悠兔机场](https://xinnongbo.com)
* [内连接(INNER JOIN)](#%E5%86%85%E8%BF%9E%E6%8E%A5inner-join)
* [等值连接](#%E7%AD%89%E5%80%BC%E8%BF%9E%E6%8E%A5)
* [非等值连接](#%E9%9D%9E%E7%AD%89%E5%80%BC%E8%BF%9E%E6%8E%A5)
* [自连接](#%E8%87%AA%E8%BF%9E%E6%8E%A5)
* [外连接](#%E5%A4%96%E8%BF%9E%E6%8E%A5)
* [左/右外连接](#%E5%B7%A6%E5%8F%B3%E5%A4%96%E8%BF%9E%E6%8E%A5)

   \_\_EOF\_\_

       - **本文作者：** [ihave2carryon](https://github.com)
 - **本文链接：** [https://github.com/ihave2carryon/p/18472524](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
