# 进阶1：基础查询

语法：

- select 查询列表 from 表名；

特点：

1. 查询列表可以是：表中的字段、常量值、表达式、函数
2. 查询的结果是一个虚拟的表格

使用数据库
USE myemployees;

##1、查询表中的单个字段
```sql
SELECT
	last_name 
FROM
	employees;
```


##2、查询表中的多个字段
```sql
SELECT
	last_name,
	salary,
	email 
FROM
	employees;
```


##3、查询表中的所有字段
```sql
SELECT
	* 
FROM
	employees;
	
```


##4、查询常量值
```sql
SELECT
	100;
SELECT
	'john';
```


##5、查询表达式
```sql
SELECT 100%98;
```



##6、查询函数
```sql
SELECT VERSION();
```



##7、起别名
1. 便于理解
2. 如果要查询的字段有重复的情况，使用别名可以区分开来
  

###方式一：使用AS
```sql
SELECT 100%98 AS 结果;
SELECT last_name AS 姓,first_name AS 名 FROM employees;
```



###方式二：使用空格
```sql
SELECT last_name 姓,first_name 名 FROM employees;
```

案例：查询salary，显示结果为out put--需要引号表示一个整体

```sql
SELECT salary AS "out put" FROM employees;
```



##8、去重

案例：查询员工表中涉及到的所有的部门编号

```sql
SELECT department_id FROM employees;
SELECT DISTINCT department_id FROM employees;
```



##9、+的作用
- java中

  1. 运算符，两个操作数都为数值型
  2. 连接符，只要有给1个操作数为字符串

- mysql中

  - 仅仅只有一个功能：运算符

    1. 两个操作数均为数值，做加法运算

       ```sql 
        SELECT 100+90; 
       ```

    2. 只要其中有一个为字符型，试图将字符型数值转换为数值型

       1. 如果转换成功，则继续做加法运算

          ```sql
          SELECT '123'+90;
          ```

       2. 如果转换失败，则将字符型数值转换为0

          ```sql
          SELECT 'john'+90;
          ```

       3. 若其中有一方为null，则结果肯定为null

          ```sql
          SELECT null+10;
          ```

案例：查询员工名和姓连接成一个字符段，并显示为姓名

```sql
SELECT CONCAT('a','b','c') AS 结果;

SELECT CONCAT(last_name,first_name) AS 姓名
FROM employees;
```

#进阶二：条件查询
/*
一、按条件表达式筛选
		条件运算符：<  >  =  <=  >=  <>
二、按逻辑表达式筛选
		逻辑运算符：and  or  not 
三、模糊查询
		like
		between and
		in
		is null
*/

USE myemployees;

##1、按条件表达式筛选

案例一：查询工资大于12000的员工信息

```sql
SELECT *
FROM employees
WHERE salary > 12000;
```

案例二：查询部门编号不等于90号的员工名字和部门编号

```sql
SELECT last_name,department_id
FROM employees
WHERE department_id <> 90;
```



##2、按逻辑表达式筛选

案例一：查询工资在10000到20000之间的员工的员工名、工资以及奖金

```sql
SELECT
		last_name,salary,commission_pct
FROM
		employees
WHERE
		salary > 10000 AND salary <20000;
```

​		

案例二：查询部门编号不是在90和110之间，或者工资高于15000的员工的信息

```sql
SELECT
		*
FROM
		employees
WHERE
		department_id < 90 OR department_id > 110 OR salary > 15000;
```



##3、模糊查询
1. like
   - 特点：一般与通配符搭配使用
   - 通配符：
     1. %    任意多个字符，包含0个字符
     2. _    任意单个字符
2. between and
3. in
4. is null    is not null

###like

案例一：查询员工名中包含字符a的员工信息

```sql
SELECT * 
FROM employees
WHERE last_name LIKE '%a%';
```

案例二：查询员工名中第三个字符为n，第五个字符为l的员工名和工资

```sql
SELECT last_name,salary
FROM	employees
WHERE last_name LIKE '__n_l%'; 
```

案例三：查询员工名中第二个字符为_的员工名

1. 方式一

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '_\_%';
```



2. 方式二------用特殊符号代表转义字符

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '_$_%' ESCAPE '$';
```



###between and

1. 可以提高语句简洁度
2. 包含两个临界值
3. 临界值不能调换顺序-----低到高
  

案例一：查询员工编号在100到120之间的员工信息

1. 方式一

```sql
SELECT *
FROM employees
WHERE employee_id >100 AND employee_id < 120;
```



2. 方式二

```sql
SELECT *
FROM employees
WHERE employee_id BETWEEN 100 AND 120;
```



###in
案例：查询员工的工种编号是IT_PROG、AD_VP、AD_PRES中的一个员工名和工种编号

1. 方式一

```sql
SELECT last_name,job_id
FROM employees
WHERE job_id = 'IT_PROG' OR job_id = 'AD_VP' OR job_id = 'AD_PRES';
```



2. 方式二

```sql
SELECT last_name,job_id
FROM employees
WHERE job_id IN ('IT_PROG','AD_VP','AD_PRES');
```



###is null

案例一：查询没有奖金的员工名和奖金率

```sql
SELECT last_name,commission_pct
FROM	employees
WHERE commission_pct IS NULL;
```

案例二：查询有奖金的员工名和奖金率

```sql
SELECT last_name,commission_pct
FROM	employees
WHERE commission_pct IS NOT NULL;
```



## 经典面试题

试问

```sql
select * from employees;
```

和

```sql
select * 
from employees 
where commission_pct like '%%' and last_name like '%%';
```

结果是否一样？并说明原因。

结果不同：如果奖金率（commission_pct）的值为null时，第二个语句将无法得出选择结果（条件不成立null无法like）

如果将and改为or，只要有一个条件中不包含null值，则结果相同。


#进阶3：进阶排序

USE myemployees;
语法：

```sql
SELECT 查询列表
FROM	表
WHERE	筛选条件
ORDER BY	排序列表（ASC   DESC）
```

特点：

1. ORDER BY子句支持单个字段、多个字段、表达式、函数、别名
2. ORDER BY子句一般放在查询语句最后，除了limit子句

案例一：查询员工信息，要求工资从高到底排下序

```sql
SELECT *
FROM	employees
ORDER BY	salary DESC;
```

案例二：查询部门编号>=90的员工信息，按入职时间的先后进行排序（添加筛选条件）

```sql
SELECT * 
FROM employees
WHERE	department_id >= 90
ORDER BY	hiredate ASC;
```

案例三：按年薪的高低显示员工的信息和年薪（按表达式表达）

```sql
SELECT *,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
FROM	employees
ORDER BY 年薪 DESC;
```

案例四：按姓名的长度显示员工的姓名和工资（按函数排序）

```sql
SELECT last_name,salary,LENGTH(last_name) 字节长度
FROM employees
ORDER BY LENGTH(last_name) DESC;
```

案例五：查询员工信息，要求先按工资降序排序，在按员工编号升序排序（按多个字段排序）

```sql
SELECT	*
FROM	employees
ORDER BY	salary DESC,employee_id ASC;
```



#进阶4：常见函数


## 一、单行函数

1. 字符函数
2. 数学函数
3. 日期函数
4. 其他函数
5. 流程控制函数

###一、字符函数

#### 1.length

length获取参数值的字节个数------一个汉字占三个字节

```sql
SELECT LENGTH('john');
SELECT LENGTH('张三丰hahaha');

SHOW VARIABLES LIKE '%char%'
```



#### 2.concat

拼接字符串-----concat

```sql
SELECT	CONCAT(last_name,'_',first_name) 姓名
FROM employees;
```



#### 3.upper,lower

大写----upper,小写----lower

```sql
SELECT UPPER('john');
SELECT LOWER('joHN');
```



#### 4.substr/subtring

索引截取----substr/substring----索引从1开始

1. 截取从指定索引处后面的所有字符

```sql
SELECT SUBSTR('李莫愁爱上了陆展元',7) out_put;
```

2. 截取从指定索引处开始指定长度的字段

```sql
SELECT SUBSTR('李莫愁爱上了陆展元',2,4) out_put;
```

案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来

```sql
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2))) out_put
FROM employees;
```



#### 5.instr

查找索引----返回字符串第一出现时的索引-----instr

```sql
SELECT INSTR('殷六。杨不悔爱上了殷六侠','殷六侠') out_put;
```



#### 6.trim

去除前后字符--------默认去除空格-----trim

```sql
SELECT TRIM('    张翠山            ') out_put;
SELECT TRIM('a' FROM 'aaaaaaaaaaa张翠山aaaaaaaaaaaa') out_put;
```



#### 7.lpad/rpad

用指定的字符填充至指定的长度------lpad--左填充-----rpad----右填充

```sql
SELECT LPAD('殷素素',10,'*') out_put;
SELECT RPAD('殷素素',12,'ab') out_put;
```



#### 8.replace

替换-----replace

```sql
SELECT REPLACE('张无忌爱上了周芷若呀周芷若','周芷若','赵敏') '花心大萝卜';
```




###二、数学函数

#### 1.round

四舍五入-------round

```sql
SELECT ROUND(1.55) out_put;
SELECT ROUND(5.87545,2);
```



#### 2.ceil,floor

2、取整函数------上去整----ceil-----下取整----------floor

```sql
SELECT	CEIL(-1.02);
SELECT	FLOOR(-9.99);
```



#### 3.truncate

截断-------truncate-------小数点后几位

```sql
SELECT TRUNCATE(1.6999999,1);
```



#### 4.mod

取余----mod（a，b）-----a-a/b*b

```sql
SELECT MOD(10,3);
```




###三、日期函数

#### 1.now

返回当前系统日期+时间-----now

```sql
SELECT NOW();
```

#### 2.curdate

返回当前系统日期，不含时间-----curdate

```sql
SELECT CURDATE();
```

#### 3.curtime

返回当前时间，不含日期-------curtime

```sql
SELECT CURTIME();
```

#### 4.year

获取指定部分的年、月、日、小时、分钟、秒

```sql
SELECT YEAR(NOW()) 年;
SELECT YEAR('1998-1-1') 年;
SELECT YEAR(hiredate) 年 FROM employees;

SELECT MONTH(NOW()) 月;
SELECT MONTHNAME(NOW()) 月；
```

#### 5.str_to_date

将字符型的日期转换为日期格式------str_to_date

```sql
SELECT STR_TO_DATE('1998-3-2','%Y-%c-%d') out_put;
```

案例一：查询入职时间为1992-4-3的员工

```sql
SELECT * FROM employees WHERE hiredate='1992-4-3';

SELECT * FROM employees WHERE hiredate=STR_TO_DATE('4-3 1992','%c-%d %Y');
```

#### 6.date_format

将日期格式的日期转换为字符格式------date_format

```sql
SELECT DATE_FORMAT(NOW(),'%y年%m月%d日') out_put;
```

案例一：查询有奖金的员工名和入职日期（xx月/xx日 xx年）

```sql
SELECT last_name,DATE_FORMAT(hiredate,'%m月/%d日 %y年') 入职日期
FROM	employees
WHERE	commission_pct IS NOT NULL;
```




###四、其他函数

####1、数据库版本
```sql
SELECT VERSION();
```

####2、查看当前数据库
```sql
SELECT DATABASE();
```

####3、查看当前用户
```sql
SELECT USER();
```


###五、流程控制函数

####1、if函数
```sql
SELECT IF(10<5,'大','小');

SELECT last_name,commission_pct,IF(commission_pct IS NULL,'没奖金，呵呵','有奖金，嘻嘻') 备注
FROM	employees;
```



#### 2、case函数

1. java中

   ```java
   switch(变量或表达式){
   					case 变量1：语句1；break；
   					···
   					default：语句n；break；
   }
   ```

   

2. mysql中

   ```sql
   case 要判断的字段或表达式
   when 常量1 then 要表达的值1或语句1；
   when 常量2 then 要表达的值2或语句2；
   ···
   else 要显示的值n或语句n；
   end
   ```

案例一：查询员工的工资，要求部门号=30，显示的工资为1.1倍；部门号=40，显示的工资为1.2倍；部门号=50，显示的工资为1.3倍；其他部门，显示为原工资

```sql
SELECT salary 原始工资,department_id,
CASE department_id
WHEN 30 THEN	salary*1.1
WHEN 40 THEN	salary*1.2	
WHEN 50 THEN	salary*1.3
ELSE		salary
END AS 新工资
FROM	employees;
```



案例二：查询员工工资的情况 ：如果工资>20000，显示A级别；如果工资>15000，显示B级别；如果工资>10000，显示C级别；否则，显示D级别

```sql
SELECT salary,
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;
```



## 二、分组函数

1. 功能：用作统计使用，又称为聚合函数或统计函数或组函数

2. 分类：

   1. sum-求和
   2. avg-平均值
   3. max-最大值
   4. min-最小值
   5. count-计数 

3. 特点：

   - sum、avg一般用于处理数值型

   - max、min、count可以处理任何类型

   - 以上分组都忽略null

   - 可以和distinct搭配

   - count函数的效率
     - MYISAM存储引擎下，COUNT(*)的效率高*
     - *INNODB存储引擎下，COUNT(*)的效率和COUNT(1)的效率差不多，比COUNT(字段)要高一些

###1、简单使用
```sql
SELECT	SUM(salary)	FROM	employees;
SELECT	AVG(salary)	FROM	employees;
SELECT	MAX(salary) FROM	employees;
SELECT	MIN(salary)	FROM	employees;
SELECT	COUNT(salary)	FROM	employees;
```



###2、支持类型
```sql
SELECT	MAX(last_name),MIN(last_name) FROM	employees;
SELECT	MAX(hiredate),MIN(hiredate)	FROM	employees;
SELECT	COUNT(commission_pct)	FROM	employees;
SELECT	COUNT(hiredate)	FROM	employees;
```



###3、和distinct搭配-----值不同
```sql
SELECT	SUM(DISTINCT salary),SUM(salary)	FROM	employees;
```



###4、count函数的详细介绍
```sql
SELECT	COUNT(salary)	FROM	employees;
SELECT	COUNT(*)	FROM	employees;
SELECT	COUNT(1)	FROM	employees;
```



# 进阶5：分组查询

语法：

```sql
select 分组函数，列（要求出现在group by的后面）
from 表
where 筛选条件
group by 分组的列表
order by 子句
```

注意：查询列表必须特殊，要求是分组函数和group by后出现的字段
		
特点：

	1. 分组查询后做条件的放在having子句中
	2. 优先使用分组前筛选--------效率高
	3. GROUP BY子句既支持单个字段分组，也支持多个字段分组
	4. 也可以添加排序（排序放在整个分组查询的最后）	

##一、简单的查询
案例一：查询每个工种的最高工资

```sql
SELECT	MAX(salary),job_id
FROM	employees
GROUP BY	job_id;
```

案例二：查询每个位置上的部门个数

```sql
SELECT	COUNT(*),location_id
FROM	departments
GROUP BY	location_id;
```




##二、添加筛选条件
案例一：查询邮箱中包含a字符的每个部门的平均工资

```sql
SELECT AVG(salary),department_id
FROM	employees
WHERE	email	LIKE	'%a%'
GROUP BY	department_id;
```

案例二：查询有奖金的每个领导手下的员工的最高工资

```sql
SELECT	MAX(salary),manager_id
FROM	employees
WHERE	commission_pct IS NOT NULL
GROUP BY	manager_id;
```



##三、添加复杂的查询条件

分组后查询------------	HAVING

案例一：查询哪个部门的员工个数>2

```sql
SELECT	department_id,COUNT(*)
FROM	employees
GROUP BY	department_id
HAVING	COUNT(*) > 2;
```

案例二：查询每个工种有奖金的员工的最高工资>12000的工种编号和最高工资

```sql
SELECT	job_id,MAX(salary) l
FROM	employees
WHERE	commission_pct IS NOT NULL
GROUP BY	job_id
HAVING	l > 12000;
```

案例三：查询领导编号>102的每个领导手下的最低工资>5000的领导编号是哪个，以及其最低工资

```sql
SELECT	manager_id,MIN(salary) mi
FROM	employees
WHERE	manager_id>102
GROUP BY	manager_id
HAVING	mi>5000;
```




##四、按表达式或函数分组
案例一：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5

```sql
SELECT	LENGTH(last_name) l,COUNT(*) c
FROM	employees
GROUP BY	l
HAVING	c>5;
```




##五、按多个字段分组
案例一：查询每个部门每个工种的员工的平均工资

```sql
SELECT	department_id	d,job_id j,AVG(salary)
FROM	employees
GROUP BY	d,j;
```




##六、添加排序
案例一：查询每个部门每个工种的员工的平均工资，并且按照平均工资的高低显示

```sql
SELECT	department_id	d,job_id j,AVG(salary) a
FROM	employees
GROUP BY	d,j
ORDER BY	a desc;
```



# 进阶6：连接查询

含义：又称多表查询，当查询的字段来自多个表是，就会用到连接查询

分类：

1. 按年代分类：
   - sql92标准
   - sql99标准
2. 按功能分类：
   - 内连接：
     - 等值连接
       - 两个表各有一（或多）个相同类型的属性，取一个属性，其中值相同的作为连接条件
     - 非等值连接
       - 两个表各有一（或多）个相同类型的属性，取一个属性，取值的大小比较作为连接条件
     - 自连接
       - 一个表连接自己，通过别名
   - 外连接：
     - 左外连接
       - 在内连接的基础上，有一些元组（或记录），留下左表的元组，右表部分用null填充
     - 右外连接
       - 在内连接的基础上，有一些元组（或记录），留下右表的元组，左表部分用null填充
     - 全外连接
       - 在内连接的基础上，有一些元组（或记录），留下各自表的元组，另一部分用null填充
   - 交叉连接
     

##sql92标准


###一、等值连接

####1、等值连接
案例一：查询员工名和对应的部门名

```sql
SELECT	last_name,department_name
FROM	employees e,departments d
WHERE	e.department_id = d.department_id;
```



####2、添加筛选
案例一：查询有奖金的员工名、部门名

```sql
SELECT	last_name,department_name
FROM	employees e,departments d
WHERE	e.department_id = d.department_id AND	commission_pct IS NOT NULL;
```



####3、添加分组
案例一：查询每个城市的部门个数

```sql
SELECT	COUNT(*) 个数,city
FROM	departments d,locations l
WHERE d.location_id = l.location_id
GROUP BY	city;
```



####4、添加排序
案例一：查询每个工种的工种名和员工的个数，并按照员工个数降序

```sql
SELECT	job_title,COUNT(*) c
FROM	jobs j,employees e
WHERE	j.job_id = e.job_id
GROUP BY	job_title
ORDER BY	c DESC;
```



####5、三表查询
案例一：查询员工名、部门名和所在的城市

```sql
SELECT	last_name,department_name,city
FROM	employees e,departments d,locations l
WHERE	e.department_id = d.department_id AND d.location_id = l.location_id
ORDER BY	department_name DESC;
```




###二、非等值连接

案例一：查询员工的工资和工资等级

```sql
SELECT	salary,grade_level
FROM	employees e,job_grades g
WHERE	salary	BETWEEN	g.lowest_sal	AND	g.highest_sal;
```




###三、自连接

案例一：查询员工名和上级的名称

```sql
SELECT	e.employee_id,e.last_name,m.employee_id,m.last_name
FROM	employees e,employees m
WHERE	e.employee_id = m.manager_id;
```



##sql99语法
语法：
		

```sql
select 查询列表
from 表一 别名 连接类型
join 表二 别名
on 连接条件
where 筛选条件
group by 分组
having 筛选条件
order by 排序列表
```

分类：

- 内连接：inner join
  - 等值连接
  - 非等值连接
  - 交叉连接
- 外连接：
  - 左外连接：left outer
  - 右外连接：right outer
  - 全外连接：full outer
- 交叉连接：cross
  		

###一、内连接
####1、等值连接
案例一：查询员工的员工名和对应的部门名

```sql
SELECT	last_name,department_name
FROM	departments d
INNER JOIN	employees e
ON	d.department_id = e.department_id;
```

案例二：查询名字中包含e的员工的员工名和其工种名（添加筛选）

```sql
SELECT	last_name,job_title
FROM		employees e 
INNER JOIN	jobs j
ON	e.job_id = j.job_id
WHERE	e.last_name LIKE '%e%';
```

案例三：查询部门个数>3的城市的城市名和其部门个数（添加筛选+分组）

```sql
SELECT	city,COUNT(*) c
FROM	departments d
INNER JOIN	locations l
ON	d.location_id = l.location_id
GROUP BY	city
HAVING	c > 3;
```

案例四：查询部门的员工个数>3的部门的部门名和员工个数，并按个数降序（添加筛选+分组+排序）

```sql
SELECT	department_name,COUNT(*) c
FROM	departments d
INNER JOIN	employees e
ON	e.department_id = d.department_id
GROUP BY	department_name
HAVING	c > 3
ORDER BY	c DESC;
```

案例五：查询员工名、部门名、工种名，并按部门名降序

```sql
SELECT	last_name,department_name,job_title
FROM	employees e
INNER JOIN	departments d ON e.department_id = d.department_id
INNER JOIN	jobs j ON e.job_id = j.job_id
ORDER BY	department_name DESC;
```




####2、非等值连接
案例一：查询员工的工资级别

```sql
SELECT	salary,grade_level
FROM	employees e
INNER JOIN	job_grades j
ON	e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

案例二：查询工资级别的个数>20的工资级别的个数，并且按照工资级别降序

```sql
SELECT	COUNT(*) c,grade_level
FROM	employees e
INNER JOIN	job_grades g
ON	salary BETWEEN g.lowest_sal AND g.highest_sal
GROUP BY	grade_level
HAVING	c > 20
ORDER BY	c;
```




####3、自连接
案例一：查询员工的名字、上级的名字

```sql
SELECT	e.last_name,m.last_name
FROM	employees e
INNER JOIN	employees m
ON	e.manager_id = m.employee_id;
```

案例二：查询姓名中包含字符k的员工的名字和上级的名字

```sql
SELECT	e.last_name,m.last_name
FROM	employees e
INNER JOIN	employees m
ON	e.manager_id = m.employee_id
WHERE	e.last_name LIKE '%k%';
```



###二、外连接
####1、左外连接
案例一：查询哪个部门没有员工

```sql
SELECT	d.*,employee_id
FROM	departments d
LEFT JOIN	employees e
ON	d.department_id = e.department_id
WHERE	e.employee_id IS NULL;
```



####2、右外连接
案例一：查询哪个部门没有员工

```sql
SELECT	d.*,employee_id
FROM	employees e
RIGHT JOIN	departments d
ON	d.department_id = e.department_id
WHERE	e.employee_id IS NULL;
```



# 进阶7：子查询

含义：出现在其他语句中的select语句，称为子查询或内查询

分类（按照子查询出现的位置）：

- select后面
  - 仅仅支持标量子查询

- from后面
  - 支持表子查询
- where或having后面
  - 标量子查询（单行单列）
  - 列子查询（单列多行）
  - 行子查询（单行多列）
- exists后面（相关子查询）
  - 表子查询（多行多列）
    									

		按照结构集的行列数不同；
						标量子查询；单行单列
						列子查询 ；单列多行
						行子查询；单行多列
						表子查询；多行多列

*/

##一、where和having后面
###1、标量子查询（单行单列）
案例一：谁的工资比Abel高

```sql
SELECT	*
FROM	employees
WHERE	salary > (
			SELECT	salary
			FROM	employees
			WHERE	last_name = 'Abel'
);
```

案例二：返回job_id与141号员工相同，salary比143号员工多的员工的姓名，job_id和工资

```sql
SELECT	last_name,job_id,salary
FROM	employees
WHERE	job_id = (
		SELECT	job_id
		FROM	employees
		WHERE	employee_id = 141
)
AND	salary > (
		SELECT	salary
		FROM	employees
		WHERE	employee_id = 143
);
```

案例三：返回公司工资最少的员工的名字，job_id和salary

```sql
SELECT	last_name,job_id,salary
FROM	employees
WHERE	salary = (
		SELECT	MIN(salary)
		FROM	employees
);
```

案例四：查询最低工资大于50号部门最低工资的部门的部门id和其最低工资

```sql
SELECT	department_id,MIN(salary)
FROM	employees
GROUP BY	department_id
HAVING	MIN(salary) > (
		SELECT	MIN(salary)
		FROM	employees
		WHERE	department_id = 50
);
```




###2、列子查询（单列多行）
案例一：返回location_id是1400或1700的部门中的所有员工姓名

```sql
SELECT	last_name
FROM	employees
WHERE	department_id IN (
		SELECT	department_id
		FROM	departments
		WHERE	location_id IN (1400,1700)
);
```

案例二：返回其他工种中比job_id为‘IT_PROG’工种任意工资低的员工的员工名、姓名、job_id以及salary

```sql
SELECT	last_name,job_id,salary
FROM	employees
GROUP BY	job_id
HAVING	salary < (
		SELECT	MAX(salary)
		FROM	employees
		WHERE	job_id = 'IT_PROG'
);
```



###3、行子查询
案例一：查询员工编号最小并且工资最高的员工信息

```sql
SELECT	*
FROM	employees
WHERE	employee_id = (
		SELECT	MIN(employee_id)
		FROM	employees
)
AND	salary = (
		SELECT	MAX(salary)
		FROM	employees
);
```



## 二、FROM后面

案例一：查询每个部门的平均工资的工资等级--------此处有问题

```sql
SELECT	ag_dep.*,g.grade_level
FROM	(
		SELECT	AVG(salary) ag,department_id
		FROM	employees
		GROUP BY	department_id
)  ag_dep
INNER JOIN	job_grades g
ON	ag_dep.salary BETWEEN	lowest_sal AND highest_sal;
```



##三、EXISTS后面
案例一：查询有员工的部门名

```sql
SELECT	department_name
FROM	departments d
WHERE	EXISTS(
		SELECT *
		FROM	employees e
		WHERE	d.department_id = e.department_id
);
```



#进阶8：分页查询
应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求

语法：

```sql
		SELECT	查询列表
		FROM	表
		JOIN TYPE JOIN	表2
		ON	连接条件
		WHERE	筛选条件
		GROUP BY	分组字段
		HAVING	筛选条件
		ORDER BY	排序字段
		LIMIT	offset，size;
```

​		
		offset------------要显示条目的起始索引（起始索引从0开始）
		size------------要显示的条目个数



特点：LIMIT语句放在查询语句的最后



案例一：查询前五条员工的信息

```sql
SELECT	*
FROM	employees
LIMIT	0,5;
```

案例二：查询第11条-第25条

```sql
SELECT	*
FROM	employees
LIMIT	10,15;
```

案例三：有奖金的员工信息，并且工资较高的前10名显示出来

```sql
SELECT	*
FROM	employees
WHERE	commission_pct IS NOT NULL
ORDER BY	salary	DESC
LIMIT	0,10;
```



# 进阶9：联合查询

UNION-----联合查询
		将多条查询语句的结构合并成一个结果

语法：

```sql
查询语句1
UNION
查询语句2
```

应用场景：要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时

案例一：查询部门编号>90或邮箱包含a的员工信息

SELECT	*	FROM	employees	WHERE	email	LIKE	'%a%'
UNION
SELECT	*	FROM	employees	WHERE	department_id>90;



#进阶10：DML语言
数据操作语言：

- 插入：insert
- 修改：update
- 删除：deleete

##一、插入语句
###插入方式一
语法：

```sql
INSERT	INTO	表名（列名，···）
VALUES	（值1，···）；
```

```sql
SELECT	*	FROM	beauty;
```



####1、插入的值的类型要与列的类型一致或兼容
```sql
INSERT	INTO	beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
```



####2、不可以为null的列必须插入值，可以为null的列如何插入值
#####方式一---------------在可以为null的列插入值null
```sql
INSERT	INTO	beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
```



#####方式二---------------省略可以为null的列
```sql
INSERT	INTO	beauty(id,NAME,sex,borndate,phone,boyfriend_id)
VALUES(14,'金星','女','1990-4-23','1388888888',9);

INSERT	INTO	beauty(id,NAME,sex,phone)
VALUES(15,'娜扎','女','1388888888');
```

3、列的顺序可以颠倒

```sql
INSERT	INTO	beauty(NAME,sex,id,phone)
VALUES('蒋欣','女',16,'110');
```

4、列和值的个数必须一致

```sql
INSERT	INTO	beauty(NAME,sex,id,phone)
VALUES('关晓彤','女',17,'110');
```

5、可以省略列名，默认所有列，而且列的顺序和表中顺序一致

```sql
INSERT	INTO	beauty
VALUES(18,'张飞','男',NULL,'119',NULL,NULL);
```




###插入方式二
语法：

```sql
INSERT	INTO	表名
SET		列名=值，列名=值，···
```



```sql
INSERT	INTO	beauty
SET	id=19,NAME='刘涛',phone='119';
```



##二、修改语句
1、修改单表的记录
语法：

```sql
UPDATE	表名
SET	列=新值，列=新值，···
WHERE	筛选语句；
```

2、修改多表的记录
语法：

- sql92语法：-----------------------仅支持内连接

  ```sql
  UPDATE	表1 别名，表2 别名
  SET	列=值，···
  WHERE	筛选条件
  AND	筛选条件；
  ```

  

- sql99语法：

  ```sql
  UPDATE	表1 别名
  INNER|LEFT|RIGHT	JOIN	表2 别名
  ON	连接条件
  SET		列=值，···
  WHERE		筛选条件；
  ```

  ```sql
  SELECT	*		FROM	beauty;
  ```

  

###1、修改单表的记录
案例一：修改beauty表中姓唐的女生的电话为13899888899

```sql
UPDATE	beauty
SET	phone = '13899888899'
WHERE	`name`	LIKE	'唐%';
```

案例二：修改boys表中id号为2的名称为张飞，魅力值为10

```sql
UPDATE	boys
SET	boyName = '张飞',userCP = 10
WHERE	id=2;
```



###2、修改多表的记录
案例一：修改张无忌的女朋友的手机号为114

```sql
UPDATE	beauty b
INNER JOIN	boys bo
ON	b.boyfriend_id = bo.id
SET		phone = '114'
WHERE	bo.boyName = '张无忌';
```

案例二：修改没有男朋友的女生的男朋友编号都为2号

```sql
UPDATE	beauty b 
LEFT JOIN	boys bo 
ON	b.boyfriend_id = b.id
SET		b.boyfriend_id = 2
WHERE		b.boyfriend_id	IS	NULL;
```




##三、删除语句
方式一：delete

语法：

1. 单表的删除

   ```sql
   DELETE	FROM	表名
   WHERE		筛选条件；
   ```

2. 多表的删除

   - sql92语法：---------------------仅支持内连接

     ```sql
     DELETE	表1的别名，表2的别名
     FROM	表1 别名，表2 别名
     WHERE	连接条件
     AND	筛选条件；
     ```

   - sql99语法：

     ```sql
     DELETE	表1的别名，表2的别名
     INNER|LEFT|RIGHT	JOIN	表2 别名
     ON	连接条件
     WHERE	筛选条件；
     ```

方式二：truncate
语法：

```sql
TRUNCATE	TABLE		表名；
```



###方式一：DELETE
###单表的删除
案例一；删除手机号一9结尾的女生信息

```sql
DELETE	FROM	beauty
WHERE	phone	LIKE	'%9';
```



###多表的删除
案例一：删除张无忌的女朋友的信息

```sql
DELETE	b
FROM	beauty b
INNER JOIN	boys bo
ON	b.boyfriend_id = bo.id
WHERE	bo.id = 1;
```

案例二：删除黄晓明以及他女朋友的信息

```sql
DELETE	b,bo
FROM	beauty b
INNER JOIN	boys bo
ON	b.boyfriend_id = bo.id
WHERE	bo.boyName = '黄晓明';
```



###方式二：TRUNCATE

案例一：将魅力值>100的男神信息删除

```sql
TRUNCATE	TABLE	boys;
```



#进阶11：DDL
数据定义语言

库和表的管理

- 库的管理：
  - 创建
  - 修改
  - 删除
    				
- 表的管理：
  - 创建
  - 修改
  - 删除
    				

##一、库的管理
### 1、库的创建

```sql
CREATE	DATABASE	库名；
```

案例一：创建库book

```sql
CREATE	DATABASE	book;
```

案例二：安全创建库book；

```sql
CREATE	DATABASE	IF	NOT	EXISTS	book;
```




###2、库的修改
案例一：更改库的字符集

```sql
ALTER	DATABASE	book	CHARACTER	SET	gbk;
```




###3、库的删除
案例一：安全删除库book

```sql
DROP	DATABASE	IF EXISTS	book;
```




##二、表的管理
###1、表的创建
语法：

```sql
CREATE	TABLE	表名（
					列名	列的类型【（长度）	约束】，
					列名	列的类型【（长度）	约束】，
					···
）
```

案例一：创建表Book

```sql
USE	book;
```

```sql
CREATE	TABLE	book(
				id INT,
				bName VARCHAR(20),
				price	DOUBLE,
				authorID	INT,
				publishDate	DATETIME
```


);

```sql
DESC	book;
```

案例二：创建表 author

```sql
CREATE	TABLE	author(
				id	int,
				au_name	VARCHAR(20),
				nation VARCHAR(10)
)
```

```sql
DESC	author;
```




###2、表的修改
```sql
ALTER	TABLE	表名	ADD|DROP|MODIFY|CHANGE	COLUMN	列名【列类型   约束】；
```

案例一：修改列名

```sql
ALTER	TABLE book 
CHANGE	COLUMN	publishDate pubDate DATETIME;
```

案例二：修改列的类型和约束

```sql
ALTER	TABLE book 
MODIFY	COLUMN pubdate TIMESTAMP;
```

案例三：添加新列

```sql
ALTER	TABLE	book
ADD	COLUMN	annual DOUBLE;
```

案例四：删除列

```sql
ALTER	TABLE	book
DROP	COLUMN	annual;
```

案例五：修改表名

```sql
ALTER	TABLE	author
RENAME	TO book_authors;
```




###3、表的删除
```sql
DROP	TABLE	IF	EXISTS	book_author;
```

```sql
SHOW	DATABASES;
```

==通用写法==

```sql
DROP	DATABASE	IF	EXISTS	旧库名；
CREATE	DATABASE	新库名；

DROP	TABLE	IF	EXISTS	旧表名；
CREATE	TABLE	新表名（）；
```




###4、表的复制

```sql
INSERT	INTO	author
VALUES	(1,'村上春树','日本'),
				(2,'莫言','中国'),
				(2,'冯唐','中国'),
				(2,'金庸','中国');
```

案例一：仅仅复制表的结构

```sql
CREATE	TABLE	copy
LIKE	author;
```

案例二：复制表的结构+数据

```sql
CREATE	TABLE	copy2
SELECT	*		FROM	author;
```

案例三：只复制部分数据

```sql
CREATE	TABLE	copy3
SELECT	id,au_name
FROM	author
WHERE	nation = '中国';
```

案例四：仅仅复制部分结构

```sql
CREATE	TABLE	copy4
SELECT	id,au_name
FROM	author
WHERE	0;
```



## ==通用写法==

```sql
DROP	DATABASE	IF	EXISTS	旧库名；
CREATE	DATABASE	新库名；

DROP	TABLE	IF	EXISTS	旧表名；
CREATE	TABLE	新表名（）；
```



#进阶12：常见约束

含义：一种限制，用于限制表格中的数据，为了保证表中的数据的准确性和可靠性
		
分类：六大约束

- not null：非空，用于保证该字段的值不能为空
  				如姓名、学号等
- default：默认，用于保证改字段有默认值
  				如性别
- primary key：主键，用于保证该字段的值都具有唯一性 ，并且非空
  				如学号
- unique：唯一，用于保证该字段的值具有唯一性，可以为空
  				如座位号
- check：检查约束【mysql中不支持】
  				如性别（是否为‘男’或‘女’）
- foreign key：外键，用于限制两个表的关系

添加约束的时机：

1. 创建表时
2. 修改表时

约束的添加分类：

1. 列级约束：六大约束都支持，但外键约束没有效果
2. 表级约束：除了非空默认，其他的都支持

```sql
CREATE	DATABASE	students;
USE	students;
```



##一、创建表时添加约束
###1、添加列级约束
```sql
DROP	TABLE	IF	EXISTS	stuinfo;
CREATE	TABLE		stuinfo(
		id INT PRIMARY KEY,
		stuName VARCHAR(20) NOT	NULL,
		gender CHAR(1) CHECK(gender = '男' OR gender = '女'),
		seat INT UNIQUE,
		age INT	DEFAULT  18,
		majorID INT REFERENCES major(id)
);
```

```sql
DESC	stuinfo;
```

```sql
DROP	TABLE	IF	EXISTS	major;
CREATE	TABLE	major(
		id INT	PRIMARY	KEY,
		majorName VARCHAR(20)
);
```



###2、添加表级约束
```sql
DROP	TABLE	IF EXISTS	stuinfo;
CREATE	TABLE	stuinfo(
		id INT,
		stuName VARCHAR(20),
		gender CHAR(1),
		seat INT,
		age INT,
		majorID int,
		
    	CONSTRAINT	pk PRIMARY	KEY(id),#主键
		CONSTRAINT  uq UNIQUE(seat),#唯一
		CONSTRAINT	ch CHECK(gender = '男' OR gender = '女'),
		CONSTRAINT	fk_stuinfo_major FOREIGN	KEY(majorID) REFERENCES			major(id)

);
```

```sql
DESC	stuinfo;
```



##二、修改表时添加约束
```sql
DROP	TABLE	IF EXISTS	stuinfo;
CREATE	TABLE	stuinfo(
		id INT,
		stuName VARCHAR(20),
		gender CHAR(1),
		seat INT,
		age INT,
		majorID INT
);
```

```sql
DESC stuinfo;
```




###1、添加非空约束
```sql
ALTER	TABLE	stuinfo
MODIFY	COLUMN stuName VARCHAR(20) NOT	NULL;
```



###2、添加默认约束
```sql
ALTER	TABLE	stuinfo
MODIFY	COLUMN age INT	DEFAULT(18);
```



###3、添加主键
####列级约束
```sql
ALTER	TABLE	stuinfo
MODIFY	COLUMN	id INT PRIMARY	KEY;
```


####表级约束
```sql
ALTER	TABLE	stuinfo
ADD	PRIMARY	KEY(id);
```



###4、添加唯一键
####列级约束
```sql
ALTER	TABLE	stuinfo
MODIFY	COLUMN	seat INT	UNIQUE;
```

####表级约束
```sql
ALTER	TABLE	stuinfo
ADD	UNIQUE(seat);
```



###5、添加外键

```sql
ALTER	TABLE	stuinfo
ADD	CONSTRAINT	fk_stuinfo_major FOREIGN	KEY(majorID) REFERENCES	major(id);
```



#进阶13：视图
含义：虚拟表，和普通表一样使用

比如：舞蹈班和普通班级的对比



##一、创建视图
```sql
语法：
create view 视图名
as
查询语句；
```



1、查询姓名中包含a字符的员工名、部门名和工种信息

```sql
CREATE VIEW myv1
AS
SELECT last_name,department_name,job_title
FROM employees e
INNER JOIN  departments d  ON  e.department_id = d.department_id
INNER JOIN  jobs j  ON  e.job_id = j.job_id;

SELECT * FROM myv1 WHERE last_name LIKE "%a%";
```

2、查询各部门的平均工资级别

```sql
CREATE VIEW myv2
AS
SELECT	AVG(salary) ag,department_id
FROM	employees
GROUP BY	department_id;

SELECT myv2.ag,g.grade_level
FROM	myv2
JOIN  job_grades g
ON	myv2.ag BETWEEN	g.lowest_sal AND	g.highest_sal;
```

3、查询平均工资最低的部门信息

```sql
SELECT *
FROM	myv2
ORDER BY	ag
LIMIT	1;
```

4、查询平均工资最低的部门名和工资

```sql
CREATE VIEW myv3
AS
SELECT *
FROM myv2
ORDER BY	ag
LIMIT 1;

SELECT d.department_name,m.ag
FROM myv3 m
JOIN	departments d
ON	m.department_id = d.department_id;
```




##二、视图的修改

###方式一：
```sql
create or replace view 视图名
as
查询语句；
```



###方式二：
```sql
alter view 视图名
as
查询语句；
```




##三、视图的查看

```sql
DESC myv3;

SHOW CREATE VIEW myv2;
```



##四、视图的删除
```sql
语法：
drop view 视图名，视图名，···；
```



```sql
DROP VIEW	myv1,myv2,myv3;
```



#进阶14：变量
/*
系统变量：
				全局变量
						跨连接，不跨重启
				会话变量
						不跨连接，连接内全局
自定义变量：
				用户变量
						连接内全局
				局部变量
						
*/

##一、系统变量
说明：变量由系统提供，不是用户定义，属于服务器层面

语法：

1. 查看所有的系统变量

   ```sql
   	show global|session variables；
   ```

2. 查看满足条件的部分系统变量

   ```sql
   show global|session variables like “%char%”;
   ```

3. 查看指定的某个系统变量的值 

   ```sql
   select @@global|session.系统变量名；
   ```

4. 为某个系统变量赋值

   - 方式一：

     ```sql
     set global|session 系统变量名 = 值;
     ```

   - 方式二：

     ```sql
     set @@global|session.系统变量名 = 值；
     ```

     

###一、全局变量
1、查看所有的全局变量

```sql
SHOW GLOBAL VARIABLES;
```

2、查看部分的全局变量

```sql
SHOW GLOBAL VARIABLES LIKE "%char%";
```

3、查看指定的全局变量的值

```sql
SELECT	@@global.autocommit;
```

4、为某个指定的全局变量赋值

```sql
SET	@@global.autocommit = 0;
```




###二、会话变量
1、查看所有的会话变量

```sql
SHOW	SESSION	VARIABLES;
```

2、查看部分的会话变量

```sql
SHOW	SESSION	VARIABLES LIKE	"%char%";
```




##二、自定义变量
说明：变量是用户自己定义的，不是由系统定义的

使用步骤：
		1、声明并初始化
				set @用户变量名 = 值；
				set @用户变量名 := 值；
				select @用户变量名 := 值；
		2、赋值
				方式一：
						set @用户变量名 = 值；
						set @用户变量名 := 值；
						select @用户变量名 := 值；
				方式二：
						select 字段 into @变量名
						from 表；								
		3、使用（查看、比较、运算）
						select @用户变量名；

### 一、用户变量

#### 声明并初始化、赋值

案例：

##### 方式一

```sql
SET	@name = 'john';
SET	@name = 100;
SET @count = 1;
```



##### 方式二

```sql
SELECT COUNT(*) INTO @count
FROM	employees;
```



#### 使用

```sql
SELECT @count;
```




###二、局部变量
作用域：仅仅在定义它单位begin end中有效

使用步骤：

1. 声明并初始化
   				declare 变量名 类型；
   				declare 变量命 类型 default 值；

2. 赋值

   - 方式一：

     ```sql
     set 用户变量名 = 值；
     set 用户变量名 := 值；
     select @用户变量名 := 值；
     ```

   - 方式二：

     ```sql
     select 字段 into 变量名
     from 表；		
     ```

     ​					

3. 使用（查看、比较、运算）
   					select 用户变量名；

###案例：

声明两个变量并赋初始值，求和，并打印

####1、用户变量
```sql
SET	@m = 1;
SET @n = 2;
SET @l = @m + @n;

SELECT @l;
```



####2、局部变量

```sql
DECLARE	a INT DEFAULT 1;
DECLARE b INT DEFAULT 2;
DECLARE c INT;

SET	c = a + b;
SELECT c;
```

