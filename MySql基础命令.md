## 1.操作数据库命令
* 登录命令`mysql -u root -p `
* 退出`exit` 或者 `\q`
* 选择数据库 `USE dbname;`
* 查看所选数据库 `SELECT DATABASE();`
* 创建数据库 `CREATE DATABASE dbname;`
* 创建数据库(使用UTF-8字符集) `CREATE DATABASE dbname CHARACTER SET UTF8;`
* 显示创建过的数据库 `SHOW DATABASES;`
* 查看数据库创建语句 `SHOW CREATE DATABASE dbname;`
* 删除数据库 `DROP DATABASE dbname`
* 修改数据库`ALERT DATABASE dbname  CHARACTER SET UTF8`
* 看数据库字符集`show create database dbname;`

## 2.操作表

* 创建表`CREATE TABLE table_name(FIELD TYPE...n);`

例子: `create table user(id int,name varchar(100),gender bit,birthday date,job varchar(255));`
* 删除表`DROP TABLE table_name`
* 显示表结构 `DESE table_name`;
* 主键约束 `id int primary key`
* 主键约束(自增) `id int primary key auto_increment`
* 唯一约束 `name varchar(100) unique`
* 非空约束 `job varchar(255) NOT NULL`
* 
相对完成例子(创建):
`create table user(id int primary key auto_increment,name varchar(100) unique,gender bit,birthday date,job varchar(255)  NOT NULL);`

* 显示创建表的语句 `show create table table_name`
* 显示库中的所有表 `show table`
* 增加一列 `ALTER TABLE USER ADD photo varchar(255);`
* 修改一列 `ALTER TABLE USER MODIFY photo varchar(100);`
* 删除一列 `ALTER TABLE USER DROP 列名字`
* 修改表名 `RENAME TABLE old_table_name TO new_table_name;`
* 修改列名 `ALTER TABLE table_name CHANGE old_col_name new_col_name VARCHAR(20);`
* 查看数据表字符集 `show create table table_name;`

## 2.操作表记录
* 增加记录 `INSERT INTO table_name(col_field_1,col_field_2) VALUES(value_1,value_2);`
* 例子: `INSERT INTO USERS(USERNAME,gender,birthday,job,photo) VALUES('张三',1,'2001-01-01','程序员','http://cxy.jpg');`
* 修改记录 `UPDATE table_name set row_name=row_value,row_name2=row_value2...`
* 修改记录(条件) `UPDATE table_name set row_name=row_value WHERE 条件表达式`
* 删除记录 `DELETE FROM table_name WHERE 条件表达式`
* 删除所有记录(一条一条删) `DELETE FROM table_name;`
* 删除所有记录(删整张表后重建表) `TRUNCATE table_name`

* 查询 `SELECT *|(col_name...) FROM table_name;`
* 排序查询 `SELECT * FROM table_name order by col asc|desc`
* 聚合函数count `SELECT COUNT(*) FROM table_name WHERE 条件表达式;`
* 聚合函数sum `SELECT SUM(row_name1),SUM(row_name2)... FROM table_name WHERE 条件表达式;`
* 聚合函数sum `SELECT SUM(row_name1+row_name2) FROM table_name WHERE 条件表达式;`
* 非空判断(空值用0替代) `SELECT SUM(IFNULL(row_name1,0)+IFNULL(row_name2,0)) FROM table_name WHERE 条件表达式;`
* 最值`SELECT MAX|MIN(row_name...) FROM table_name`
```shell
# __成绩查询例子__
CREATE TABLE grade(id int primary key auto_increment,name varchar(20),eng double,pe double,math double);
INSERT INTO GRADE VALUES(null,"张三",92,60,45);
INSERT INTO GRADE VALUES(null,"张三疯",89,61,90);
INSERT INTO GRADE VALUES(null,"李四",78,90,89);
INSERT INTO GRADE VALUES(null,"王五",87,67,45);


#查询所有学生的成绩
SELECT * FROM GRADE;
#查询所有姓名和数学成绩
SELECT NAME,MATH FROM GRADE;
#过滤表中重复数据
SELECT DISTINCT MATH FROM GRADE;
#分数加5分显示
SELECT MATH+10 FROM GRADE;
#统计每个人的总分
SELECT NAME,ENG+PE+MATH FROM GRADE;
#使用别名
SELECT NAME,ENG+PE+MATH AS 总分 FROM GRADE;
#查询张三的所有成绩
SELECT * FROM GRADE WHERE NAME='张三';
#查询总分大于200的
SELECT NAME,PE+MATH+ENG AS 总分 FROM GRADE WHERE ENG+MATH+PE > 200;
#区间查询
SELECT NAME,ENG AS 英语成绩 FROM GRADE WHERE ENG BETWEEN 80 AND 100;
#范围查询(列表为范围进行查询)
SELECT NAME,ENG AS '英语成绩(符合指定列表)' FROM GRADE WHERE ENG IN(92,87,78);
#模糊查询
SELECT * FROM GRADE WHERE NAME LIKE "张%";
SELECT * FROM GRADE WHERE NAME LIKE "张_";
SELECT * FROM GRADE WHERE NAME LIKE "张__";
#多条件
SELECT * FROM GRADE WHERE PE>60 AND MATH>80;
#排序查询
SELECT * FROM GRADE ORDER BY PE ASC;
SELECT * FROM GRADE ORDER BY PE DESC;
SELECT NAME 姓名,PE+MATH+ENG 总成绩 FROM GRADE WHERE NAME LIKE '张%' ORDER BY 总成绩 DESC;
SELECT NAME AS 姓名,PE+MATH+ENG AS 总成绩 FROM GRADE WHERE NAME LIKE '张%' ORDER BY 总成绩 DESC;
#平均数
SELECT SUM(MATH)/COUNT(MATH) FROM GRADE;
SELECT AVG(IFNULL(MATH,0)) FROM GRADE;
#最值
SELECT MATH+PE+ENG AS 总分ROM GRADE;
SELECT MAX(MATH+PE+ENG) AS '总分(最大值)' FROM GRADE;
SELECT MIN(MATH+PE+ENG) AS '总分(最小值)' FROM GRADE;

```

* 分组查询
`SELECT row_name1 FROM ORDERS WHERE 条件表达式1 GROUP BY row_name2 having 条件表达式2`

## 3.备份恢复
* 备份 `mysqldump -u root -p dbName > filePath`
* 恢复 `mysqldump -u root -p dbName < filePath`
* 恢复2 `mysql>source filePath`
* 
### 4.字符集
```
CREATE DATABASE db_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE DATABASE 的语法：
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
[create_specification [, create_specification] ...]
create_specification:
[DEFAULT] CHARACTER SET charset_name
| [DEFAULT] COLLATE collation_name

更改数据库的字符编码
ALTER DATABASE db_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci
```