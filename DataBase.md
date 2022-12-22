# DDL
### 数据库操作
* query
```sql
SHOW DATABASES; -- query all databases
SELECT DATABASE(); -- query current databases
```
* create
```sql
CREATE DATABASE [IF NOT EXISTS] _NAME 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]
```
* delete
```sql
DROP DATABASE[IF EXISTS] _NAME 数据库名
```
* use
```sql
USE _NAME 数据库名
```
### 数据库表操作
* query
```sql
SHOW TABLES; -- query current database's tables
DESC table_name; -- query table structure
SHOW CREATE TABLE 表名; -- query exists table structure in detail
```
* create
```sql
CREATE TABLE table_name(
    字段1 字段1类型[COMMENT 字段1注释],
    字段2 字段2类型[COMMENT 字段2注释],
    字段3 字段3类型[COMMENT 字段3注释],
    ......
    字段n 字段n类型[COMMENT 字段n注释] -- don need to add the ''
)[COMMENT 表注释];
```
> create table test_create(
    id int comment 'id',
    name varchar(50) comment 'name',
    age int comment 'age',
    gender varchar(1) comment 'gender'
) comment 'user_table';
* change
```sql
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
ALTER TABLE 表名 MODIFY 字段名 新类型(长度);
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新类型(长度) [COMMENT 注释] [约束];
ALTER TABLE 表名 DROP 字段名; -- delete 字段
ALTER TABLE 表名 RENAME TO 新表名; -- rename
DROP TABLE [IF EXISTS] 表名; -- delete
TRUNCATE TABLE 表名; -- 会重新创建该表
```
### 数据类型
| Type       | Size                             | Description                               |
|------------|----------------------------------|-------------------------------------------|
| TINYINT    | 1 byte                           |                                           |
| SMALLINT   | 2 bytes                          |                                           |
| MEDIUMINT  | 3 bytes                          |                                           |
| INT/INTGER | 4 bytes                          |                                           |
| BIGINT     | 8 bytes                          |                                           |
| FLOAT      | 4 bytes                          |                                           |
| DOULE      | 8 bytes                          |                                           |
| DECIMAL    | 小数值，依赖M(精度)和D(标度)的值 |                                           |
| CHAR       | 0-255 bytes                      | const string                              |
| VARCHAR    | 0-65535 bytes                    | string                                    |
| TINYBLOB   | 0-255 bytes                      |                                           |
| TINYTEXT   | 0-255 bytes                      |                                           |
| BLOB       | 0-65535 bytes                    | binary long text data                     |
| TEXT       | 0-65535 bytes                    | long text data                            |
| MEDIUMBLOB | 0-16777215 bytes                 |                                           |
| MEDIUMTEXT | 0-16777215 bytes                 |                                           |
| LONGBLOB   | 0-4294967295 bytes               |                                           |
| LONGTEXT   | 0-4294967295 bytes               |                                           |
| DATE       | 3                                | 1000-01-01-01 ~ 9999-12-31                |
| TIME       | 3                                | -838:59:59 ~ 838:59:59                    |
| YEAR       | 1                                | 1901 ~ 2155                               |
| DATETIME   | 8                                | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 |
| TIMESTAMP  | 4                                | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 |

> score double(4,1) 精度指总位数，标度指小数位

# DML
* insert
```sql
INSERT INTO 表名(字段1，字段2， ...) VALUES(值1,值2,...);
INSERT INTO 表名VALUES(值1,值2,...);
INSERT INTO 表名(字段1，字段2， ...) VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...);
INSERT INTO 表名VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...);
```
* change
```sql
UPDATE 表名 SET 字段名1=值1,字段名2=值2,...[WHERE 条件] -- 不指明条件则修改整张表
```
* delete
```sql
DELETE FROM 表名 [WHERE 条件]
```
# DQL
* select query
```sql
SELECT 字段列表
FROM 
    表名列表
WHERE -- 分组前进行过滤,不可对聚合函数过滤
    条件列表
GROUP BY 
    分组字段列表
HAVING -- 分组后对结果进行过滤,可对聚合函数过滤
    分组后条件列表
ORDER BY 
    排序字段列表 -- (字段1 排序方式1, 字段2 排序方式2)
    ASC:升序
    DESC：降序
LIMIT 
    分页参数(起始索引，查询记录数) -- 索引默认从0开始
DISTINCT 字段列表 -- 去重操作
LIKE _代表占位符，%代表任意一个字符
```
> select distinct age from worker;
| function | Function |
|----------|----------|
| count    | 统计数量 |
| max      | 最大值   |
| min      | 最小值   |
| avg      | 平均值   |
| sum      | 求和     |

# DCL
* query user
```sql
USE mysql;
SELECT * FROM user;
```
* create localhost user(use the % to let user access the database on any host)
```sql
CREATE USER 'name'@'localhost' IDENTIFIED BY 'password';
```
* change the user's password
```sql
ALTER USER 'name'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
```
* delete user
```sql
DROP USER 'name'@'localhost';
```
* query permissions
```sql
SHOW GRANTS FO 'name'@'localhost';
```
* give permissions
```sql
GRANT 权限列表 ON 数据库名.表名 TO 'name'@'localhost';
```
* delete permissions
```sql
REVOKE 权限列表ON 数据库名.表名 FROM 'name'@'localhost'; 
```

| permissions        | description              |
|--------------------|--------------------------|
| ALL,ALL PRIVILEGES | all permissions          |
| SELECT             | query permissions        |
| INSERT             | insert permissions       |
| UPDATE             | modify permissions       |
| DELETE             | delete permissions       |
| ALTER              | MODIFY table permissions |
| DROP               | delete database/table    |
| CREATE             | create database/table    |

# function
| string                   | description                                     |
|--------------------------|-------------------------------------------------|
| CONCAT(s1,s2,...sn)      | 字符串拼接                                      |
| LOWER(str)               | 字符串转小写                                    |
| UPPER(str)               | 字符串转大写                                    |
| LPAD(str,n,pad)          | 左填充                                          |
| RPAD(str,n,pad)          | 右填充                                          |
| TRIM(str)                | 去掉字符串头部和尾部的空格                      |
| SUBSTRING(str,start,len) | 返回从字符串str从start位置起的len个长度的字符串 |

> select lpad('01',5,'-');

| number     | description                        |
|------------|------------------------------------|
| CEIL(x)    | 向上取整                           |
| FLOOR(x)   | 向下取整                           |
| MOD(x,y)   | 返回x/y的摸                        |
| RAND()     | 返回0～1内的随机数                 |
| ROUND(x,y) | 求参数x的四舍五入的指，保留y位小数 |

> select roun(2.3444,2);

| date                              | description                                     |
|-----------------------------------|-------------------------------------------------|
| CURDATE()                         | 返回当前日期                                    |
| CURTIME()                         | 返回当前时间                                    |
| NOW()                             | 返回当前日期和时间                              |
| YEAR(date)                        | 获取date的年份                                  |
| MONTH(date)                       | 获取date的月份                                  |
| DAY(date)                         | 获取date的日期                                  |
| DATE_ADD(date,INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr的时间值 |
| DATEDIFF(date1,date2)             | 返回起始时间date1与结束时间date2之间的天数      |

> select name,datediff(curdate(),entrydate) entrydays from worker order by entrydays asc;

| process                                                    | description                                          |
|------------------------------------------------------------|------------------------------------------------------|
| IF(value,t,f)                                              | 如果value为true，则返回t,否则为f                     |
| IFNULL(value1,value2)                                      | 如果value1不为空，返回value1，否则返回value2         |
| CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END | 如果expr等于val1，返回res1，...否则返回default默认值 |

# constraint
| constraint | description                                            | key         |
|------------|--------------------------------------------------------|-------------|
| 非空约束   | 限制该字段的数据不为null                               | NOT NULL    |
| 唯一约束   | 保证该字段的数据唯一                                   | UNIQUE      |
| 主键约束   | 主键是一行数据的唯一标识                               | PRIMARY KEY |
| 默认约束   | 保存数据时，如果未指定该字段的指，则用默认值           | DEFAULT     |
| 检查约束   | 保证字段之满足某一个条件                               | CHECK       |
| 外键约束   | 使得两张表的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |

> create table user(id int primary key auto_increment comment'主键', name varchar(10) not null unique comment '姓名',age int comment '年龄' check (age > 0 && age <= 120) ,status char(1) default '1' comment '状态',gender char(1) comment '性别') comment '用户表';

* add foreign key
```sql
CREATE TABLE 表名(
    字段名 数据类型,
    ...
    [CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERNCES 主表(主表列名)
);
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERNCES 主表(主表列名);
```
* delete foreign key
```sql
ALTER TABLE 表名DROP FOREIGN KEY 外键名称;  
```
