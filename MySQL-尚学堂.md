# 目录

[TOC]

# 内容

## 1. DQL语言

### 1.1 基础查询

**基本语法**

```mysql
SELECT 查询列表 FROM 表名;
```

>  （1）查询列表可以是：表中的字段、常量值、表达式、函数
>
> （2）查询的结果是一个虚拟的表格

**查询语句**

```mysql
1.查询表中的单个字段
 SELECT name FROM employee;
 
2.查询表中的多个字段
SELECT name,salary,email FROM employee;

3.查询表中的所有字段
SELECT * FROM employee;

4.查询常量值
SELECT 100;
SELECT 'john';

5.查询表达式
SELECT 100*98;
SELECT 100%98;

6.查询函数
SELECT VERSION();

7.为字段起别名
SELECT 100%98 AS 别名;
SELECT last_name AS 姓氏, first_name AS 名字 FROM employee;  // 使用AS
SELECT last_name 姓氏, first_name 名字 FROM employee;  // 使用空格

8.去重
# 案例：查询员工表中涉及到的所有的部门编号
SELECT DISTINCT department_id FROM employee;  // 去重使用distinct关键字

9.+号的作用
/*
* mysql中的+只有一个功能：运算符
* select 100+90；两个操作数都为数值型，则做加法运算
* select '123'+90；其中一方为字符型，试图将字符型数值转换为数值型，如果转换成功，则继续做加法运算
* select 'john'+90；其中一方为字符型，试图将字符型数值转换为数值型，如果转换失败，则将字符型数值转换为0
* select null+90；只要其中一方为null，结果一定为null
*/
# 案例：查询员工名和姓连接成一个字段，并显示为姓名
SELECT CONCAT(last_name, first_name) AS 姓名 FROM employee;
```



### 1.2 条件查询

**基本语法**

```mysql
SELECT 查询列表 FROM 表名 WHERE 筛选条件;
/*
* 分类：
* 1.按条件表达式筛选
* 	条件运算符：> < = <> >= <=
*
* 2.按逻辑表达式筛选
* 	逻辑运算符：&& || ! 
*            and or not
*
* 3.模糊查询
* 	like 
*   	特点：
*     			一般和通配符搭配使用
*     通配符：
*						%：表示任意多个字符，包含0个字符
*           _：表示任意单个字符
*		between and
*   	注意事项：
*            （1）between and包含两边的临界值
*     			 （2）between and的两个临界值顺序不能颠倒
*		in
*   	用途：
*     	判断某字段的值是否属于in列表中的某一项
*     注意事项：
*				（1）in列表中的各个值要一致或兼容（'123'->123）
*				（2）in列表中的各个值不能使用通配符
*		is null和is not null
*   		=和<>不能用于判断null值
*       is null和is not null （is仅可以用于判断null值，不可以用于比较普通数值）
*/
```



**查询语句**

```mysql
1.按条件表达式筛选
# 案例1：查询工资>12000的员工信息
SELECT * FROM employee WHERE salary > 12000;
# 案例2：部门编号不等于90号的员工名和部门编号
SELECT name, department_id FROM employee WHERE department_id<>90;

2.按逻辑表达式筛选
# 案例1：查询工资在10000到20000之间的员工名、工资和奖金
SELECT name,salary,commision_pct FROM employee WHERE salary>=10000 AND salary<=20000;
# 案例2：查询部门编号不是在90到110之间，或工资高于15000的员工信息
SELECT * FROM employee WHERE department_id<90 OR department_id>110 OR salary>15000;
SELECT * FROM employee WHERE NOT(department_id>=90 AND department_id<=110) OR salary>15000;

3.模糊查询-like
# 案例1：查询员工名中包含字符a的员工信息
SELECT * FROM employee WHERE name LIKE '%a%';  // %为通配符，代表任意个数字符，包含0个字符
# 案例2：查询员工名中第3个字符为e，第5个字符为a的员工名和工资
SELECT name,salary FROM employee WHERE name LIKE '__e_a%';  // _为通配符，代表任意单个字符
# 案例3：查询员工名中第2个字符为_的员工名
SELECT name FROM employee WHERE name LIKE '_\_%';  // “\”为mysql的转义字符
SELECT name FROM employee WHERE name LIKE '_$_%' ESCAPE '$';  // 自定义“$”为转义字符，也可定义其他字符

4.between and
# 案例1：查询员工编号在100到120之间的员工信息
SELECT * FROM employee WHERE employee_id>=100 AND employee_id<=200;
SELECT * FROM employee WHERE employee_id BETWEEN 100 AND 200;

5.in
# 案例1：查询员工的工种编号是IT_PROG、PM、UI中的一个的员工信息
SELECT * FROM employee WHERE job_id='IT_PROG' OR job_id='PM' OR job_id='UI';
SELECT * FROM employee WHERE job_id IN ('IT_PROG', 'PM', 'UI');

6.is null和is not null
# 案例1：查询没有奖金的员工名和奖金率
SELECT name,commision_pct FROM employee WHERE commision_pct IS NULL;
# 案例2：查询有奖金的员工名和奖金率
SELECT name,commision_pct FROM employee WHERE commision_pct IS NOT NULL;

7.安全等于 <=> 可以用于判断普通类型的值和null值
# 案例1：查询没有奖金的员工名和奖金率
SELECT name,commision_pct FROM employee WHERE commision_pct <=> NULL;
```



### 1.3 排序查询

**基本语法**

```mysql
SELECT 查询列表 FROM 表名 (WHERE 筛选条件) ORDER BY 排序列表 (ASC|DESC)
/**
* 注意事项：
*			（1）ASC代表升序，DESC代表降序，默认为ASC
*/
```



**查询语句**

```mysql
# 案例1：查询员工信息，要求工资从高到低排序
SELECT * FROM employee ORDER BY salary DESC;
# 案例2：查询员工信息，要求工资从低到高排序
SELECT * FROM employee ORDER BY salary ASC;
SELECT * FROM employee ORDER BY salary;
# 案例3：查询部门编号>=90的员工信息，按入职时间的先后进行排序【筛选+排序】
SELECT * FROM employee WHERE department_id>=90 ORDER BY hiredate ASC;  // 升序
SELECT * FROM employee WHERE department_id>=90 ORDER BY hiredate DESC;  // 降序
# 案例4：按照年薪降序显示员工的信息和年薪【按表达式排序】（table中没有年薪，只有月薪salary和奖金commision_pct）
SELECT *, salary*12*(1+IFNULL(commision_pct, 0)) 年薪 FROM employee ORDER BY salary*12*(1+IFNULL(comision_pct, 0)) DESC;
# 案例5：按照年薪降序显示员工的信息和年薪【按照别名排序】
SELECT *, salary*12*(1+IFNULL(commision_pct, 0)) 年薪 FROM employee ORDER BY 年薪 DESC;
# 案例6：按照姓名的长度降序显示员工的姓名和工资【按照函数排序】
SELECT LENGTH(name) 姓名长度,name,salary FROM employee ORDER BY LENGTH(name) DESC;
# 案例7：查询员工信息，先按员工工资升序，工资一样时再按员工编号降序【按多个字段排序】
SELECT * FROM employee ORDER BY salary ASC, employee_id DESC;
```



## 2. DML语言

