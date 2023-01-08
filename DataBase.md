# DDL(数据库定义语言)
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

# DML(数据操纵语言)
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
# DQL(数据查询语言)
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

# DCL(数据控制语言)
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
SHOW VARIABLES LIKE 'validate_password%'; -- 查看密码配置
ALTER USER 'name'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
```
* delete user
```sql
DROP USER 'name'@'localhost';
```
* query permissions
```sql
SHOW GRANTS FOR 'name'@'localhost';
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

# multi-table query

## one-to-many
> 多的方面构建外键

## many to many
> 建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

## one-to-one
> 用于单表拆分，基础字段和其他详细字段分开存放

## query
### 连接查询
#### 内连接：查询交集部分数据
```sql
-- 隐式内连接
SELECT 字段列表 FROM 表1,表2 WHERE 条件 ...;
-- 显式内连接
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件 ...;
```
#### 外连接：
* 左外连接：查询左表所有数据，以及两张表交集部分的数据
```sql
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
```
* 右外连接：查询右表所有数据，以及两张表交集部分的数据
```sql
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
```
#### 自连接：当前表与自身的连接查询，自连接必须使用表别名
    ```sql
    SELECT 字段列表 FROM 表1 别名1 JOIN 表2 别名2 ON 条件...;
    ```
### 联合查询(字段列表需完全相同)
```sql
SELECT 字段列表 FROM 表1...
UNION [ALL] --- 去掉ALL实现去重
SELECT 字段列表 FROM 表1...;
```
### 子查询(嵌套SELECT语句)
```sql
SELECT 字段列表 FROM 表1 WHERE 字段1 = (SELECT 字段1 FROM 表2);
```

#### 标量子查询(子查询结果为单个值)

常用的操作符: =, <>, >, >=, <, <=

#### 列子查询(子查询结果为一列)

    常用的操作符: IN, NOT IN, ANY, SOME, ALL
    SELECT 字段列表 FROM 表1 WHERE 字段1 > ANY (SELECT 字段1 FROM 表2 WHERE 条件...);
> 行子查询(子查询结果为一行)

    常用的操作符: =, <>, IN, NOT IN
    SELECT 字段列表 FROM 表1 WHERE (字段1,字段2) = (SELECT 字段1,字段2 FROM 表1 WHERE 条件...);
> 表子查询(子查询结果为多行多列)

    常用的操作符: IN
    SELECT 字段列表 FROM 表1 WHERE (字段1,字段2) IN (SELECT 字段1,字段2 FROM 表1 WHERE 条件...);

# transaction
## operation
* 查看/设置事务提交方式
```sql
SELECT @@autocommit; -- 1
SET @@autocommit=0; -- manually
```
* 开启事务
```sql
START TRANSACTION / BEGIN;
```
* 提交事务
```sql
COMMIT;
```
* 回滚事务
```sql
ROLLBACK;
```

## features
* 原子性(Atomicity):事务是不可分割的最小操作单元，要么全部成功，要么全部失败
* 一致性(Consistency):事务完成时，必须使所有的数据都保持一致
* 隔离性(Isolation):数据库系统提供的隔离机制，保证事务在不受外界并发操作影响的独立环境下运行
* 持久性(Durability):事务一旦提交或回滚，此改变就是永久的

## problems/phenomenon
* 赃读:在一个事务中读到另外一个事务还没有提交的数据
* 不可重复读:在一个事务中先后读取同一条记录，但两次读取的数据不同
* 幻读:一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在

## isolation level
| isolation level          | 脏读 | 不可重复读 | 幻读 |
|--------------------------|------|------------|------|
| Read uncommitted         | 1    | 1          | 1    |
| Read committed           | 0    | 1          | 1    |
| Repeatable Read(default) | 0    | 0          | 1    |
| Serializable(可序列化)   | 0    | 0          | 0    |

* 查看事务隔离级别
```sql
SELECT @@TRANSACTION_ISOLATION / @@TX_ISOLATION;
```
* 设置事务隔离级别
```sql
SET [SESSION GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE};
```

# MySQL体系结构

### 客户端

### 服务端

#### 连接层

#### 服务层

#### 引擎层
```sql
CREATE TABLE table_name(
    字段1 字段1类型[COMMENT 字段1注释],
    字段2 字段2类型[COMMENT 字段2注释],
    字段3 字段3类型[COMMENT 字段3注释],
    ......
    字段n 字段n类型[COMMENT 字段n注释] -- don need to add the ''
)ENGINE = INNODB [COMMENT 表注释];

SHOW ENGINES; -- 查看数据库支持的存储引擎
```

**`InnoDB:`**

DML操作遵循ACID模型，支持事务

行级锁，提高并发访问性能

支持外键FOREIGN KEY 约束，保证数据的完整性和正确性

1. TableSpace:表空间
> idb文件,一个mysql实例可以对应多个表空间，用于存储记录，索引等数据
2. Segment:段
> 数据段(Lead node Segment),索引段(Non-leaf node Segment),回滚段(Rollback Segment),InnoDB是索引组织表，数据段就是B+树的叶子节点，索引段就是B+树的非叶子节点
3. Extent(1M):区
> 一个区中默认有连续64个页
4. Page(16K):页
> InnoDB存储引擎磁盘管理的最小单元，每次InnoDB引擎向磁盘申请4-5个区
5. Row:行
> InnoDB存储引擎每一行的数据
**`MyISAM`**

不支持事务，不支持外键

支持表锁，不支持行锁

访问速度快

**`Memory:`**

内存存放

hash索引

| 特点             | InnoDB            | MyISAM     | Memory     |
| :--------------: | :---------------: | :--------: | :--------: |
| 存储限制         | 64TB              | 有         | 有         |
| 事务安全         | 支持              |            |            |
| 锁机制           | 行锁              | 表锁       | 表锁       |
| B+tree索引       | 支持              | 支持       | 支持       |
| Hash索引         | -(自适应)         | -          | 支持       |
| 全文索引         | 支持(5.6之后)     | 支持       | -          |
| 空间使用         | 高                | 低         | N/A        |
| 内存使用         | 高                | 低         | 中等       |
| 批量插入速度     | 低                | 高         | 高         |
| 支持外键         | 支持              | -          | -          |

### 存储层

## 索引(index)
**帮助MySQL高效获取数据的数据结构(有序)** 
| 索引结构            | 描述                           |
| B+Tree索引          | 最常见的索引类型               |
| Hash索引            | 精确匹配索引，不支持范围查询   |
| R-tree(空间索引)    | MyISAM引擎的一个特殊索引类型   |
| Full-text(全文索引) | 通过建立倒排索引，快速匹配文档 |

### B-Tree(多路平衡查找树)
```text
以一颗最大度数(子节点个数)为5的b-tree为例(每个节点最多存储4个key,5个指针)
```
### B+Tree(MySQL)
    * 所有的数据都会出现在叶子节点
    * 叶子节点构成一个双向链表(原本为单向链表)
### Hash
    * 只能用于对等比较(=,in),不支持范围查(between,>,<,...)
    * 无法利用索引完成排序操作
    * 查询效率高，通常只需要一次检索，效率高于B+Tree索引

```text
采用一定的hash算法，将键值换算成新的hash值，映射到对应的的槽位上，然后存储在hash表中
如果有多个键值都映射到同一个槽位上，就产生了hash冲突，可以通过链表解决
```

### 分类

| 分类     | 含义                                   | 特点                     | 关键字   |
|----------|----------------------------------------|--------------------------|----------|
| 主键索引 | 针对于表中主键的索引                   | 默认自动创建，只能有一个 | PRIMARY  |
| 唯一索引 | 避免同一个表中某数据列中的值重复       | 可以有多个               | UNIQUE   |
| 常规索引 | 快速定位特定数据                       | 可以有多个               |          |
| 全文索引 | 查找文本中的关键词，而不是比较索引的值 | 可以有多个               | FULLTEXT |

* InnoDB
    * InnoDB的指针占用6个字节

| 分类     | 含义                                                       | 特点               |
|----------|------------------------------------------------------------|--------------------|
| 聚集索引 | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有，且只有一个 |
| 二级索引 | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个       |
* 聚集索引选取规则：
    * 如果存在主键，主键索引就是聚集索引
    * 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引
    * 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引
```sql
SELECT * FROM USER WHERE name = 'xxx';

回表查询(此行为说明如果要获取整行数据，以主键为条件查询是效率最高的一种查询方式):
1. 根据WHERE关键字由name的二级索引查询出对应的主键值
2. 再根据主键值由主键的聚集索引查询这一行的数据
```

### 索引使用

* 最左前缀法则
**如果索引了多列(联合索引),要遵循最左前缀法则--查询从索引的最左列开始，且不跳过索引中的列,如果跳跃某一列，索引将部分失效(后面的字段索引失效)** 

* 范围查询
**联合索引中，出现范围查询(>,<),范围查询右侧的列索引失效** 

* 覆盖索引(需要查询返回的数据在索引结构中可以直接找到，不需要在回表)

* 前缀索引(索引若为很长的字符串时，只将字符串的一部分前缀建立索引，节约索引空间)
```sql
CREATE INDEX idx_xxxx ON table_name(COLUMN(n));
```
* 单列索引优先级比联合索引更高,但联合索引对于多个查询条件的查询效率更高

### 索引失效

* 索引列运算
* 字符串不加引号
* 模糊查询(如果是尾部模糊，索引不会失效，如果是头部模糊，索引会失效)
* or连接的条件(用or连接的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都会失效)
* 数据分布影响(如果MySQL评估使用索引比全表慢，则不使用索引)
* SQL提示(人为提示来达到优化操作的目的)
    * use index(建议)
    * ignore index(忽略)
    * force index(强制)

### 语法
* create
```sql
CREATE [UNIQUE | FULLTEXT] INDEX index_name ON table_name (index_col_name,...);
```
* query
```sql
SHOW INDEX FROM table_name[\G];
```
* delete
```sql
DROP INDEX index_name ON table_name;
```

## SQL性能分析

* SQL执行频率
```sql
SHOW [SESSION | GLOBAL] STATUS; -- 查询服务器状态信息
SHOW GLOBAL STATUS LIKE 'COM_______'; -- 7 个下划线
```

* 慢查询日志

**慢查询日志记录了所有执行时间超过指定参数(long_query_time,单位:秒，默认10秒)的所有SQL语句的日志,需要在MySQL的配置文件(/etc/my.cnf)=配置** 

```text
slow_query_log=1 -- 开启
long_query_time=2 -- 设置时间
```

*profile详情
```sql
SELECT @@have_profiling; -- 查看profiling
SET profiling = 1; -- 开启
SHOW PROFILES; -- 查看每一条SQL语句的耗时情况
SHOW PROFILE FOR QUERY QUERY_ID; -- 查看指定QUERY_ID的各SQL语句各个阶段的耗时情况
SHOW PROFILE CPU FOR QUERY QUERY_ID -- 查看指定QUERY_ID的各SQL语句CPU的使用情况
```

*explain执行计划
```sql
EXPLAIN/DESC SELECT 字段列表 FROM 表名 WHERE 条件;
```
**EXPLAIN或者DESC获取MySQL如何执行SELECT语句，包括了在SELECT语句执行过程中表如何连接和连接的顺序** 
    * id
        * SELECT查询的序列号，表示查询中执行SELECT子句或者是操作表的顺序(id相同，执行顺序从上往下，id不同值越大，越先执行)
    * select_type
        * 表示SELECT的类型，常见的取值有SIMPLE(简单表，即不使用表连接或子查询)，PRIMARY(主查询，即外层的查询)，UNION(UNION中的第二个或者后面的查询语句)，SUBQUERY(SELECT/WHERE之后包括了子查询)
    * type
        * 表示连接类型，性能由好到差的连接类型为NULL,system,const,eq_ref,ref,range,index,all
    * possible_key
        * 显示可能应用在这张表上的索引，一个或多个
    * key
        * 实际使用的索引
    * key_len
        * 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度
    * rows
        * MySQL认为必须要执行查询的行数，在InnoDB引擎的表中，是一个估计值
    *filtered
        * 表示返回结果的行户占需读取行数的百分比，该值越大越好

## MySQL优化
### insert优化
    * 批量插入
    * 手动提交事务
    * 主键顺序插入
### 大批量插入数据(load)
```sql
-- 连接客户端
mysql --local-infile -u root -p
-- 设置全局参数，开启从本地加载文件导入数据的开关
set global local_infile=1;
-- 执行load命令加载数据
load data local infile 'ABSOLUTE_PATH' into table table_name fields terminated by ',' line terminated by '\n';
```

### 主键
**在InnoDB存储引擎中，表数据根据主键顺序组织存放----索引组织表** 
* 页分裂(出现在主键乱序插入的情况下)
    * 页可以为空，也可以填充一半，也可以填充全部，每个页包含了2-N行数据，根据主键排列
* 页合并
    * 当删除一行记录使，只是记录被标记(flaged)为删除并且空间变得允许被其他记录声明使用
    * 当页中删除的记录达到MERGE_THRESHOLD(默认为页的50%)，InnoDB会开始寻找最靠近的页来决定是否合并来节省空间
* 设计原则
    * 尽量降低主键长度
    * 尽量选择顺序插入，使用auto_increment自增主键
    * 尽量不使用UUID做主键
    * 尽量不修改主键

### order by
* using filesort
    * 通过表的索引或全表扫描，读取满足条件的数据行,然后在排序缓冲区sort buffer进行排序操作，不通过索引直接返回排序结果的排序为FileSort排序
* using index
    * 通过有序索引顺序扫描直接返回有序数据，不需要额外排序
* 设计规则
    * 建立合适的索引
    * 尽量使用覆盖索引
    * 索引创建时可以指定顺序规则(ASC/DESC)
    * 适当增大排序缓冲区的大小sort_buffer_size(默认256K)

### group by
    * 建立合适的索引

### limit
    * 创建覆盖索引加子查询进行优化

### count
**cout(字段)<count(主键 id)<count(1)=count(*)** 
    * 自己计数

### update
**尽量对有索引的字段进行查询避免行锁升级为表锁** 

## 视图
**视图是一种虚拟存在的表，其中的数据并不实际存在，只保存了查询SQL逻辑，不保存查询结果** 

* create
```sql
CREATE [OR REPLACE] VIEW view_name AS SELECT语句 [WITH [CASCADED | LOCAL] CHECK OPTION]
```

* query
```sql
SHOW CREATE VIEW view_name;
SELECT * FROM view_name;
```
* change
```sql
CREATE [OR REPLACE] VIEW view_name AS SELECT语句 [WITH [CASCADED | LOCAL] CHECK OPTION];
ALTER VIEW view_name AS SELECT语句 [WITH [CASCADED | LOCAL] CHECK OPTION];
```

* drop
```sql
DROP VIEW [IF EXISTS] view_name;
```

### 检查选项
**默认为CASCADED** 
> CASCADED(级联):对视图进行操作时会检查当前视图及子视图所有的过滤条件
> LOCAL:对视图进行操作时会检查当前视图及子视图中存在检查条件的语句中的过滤条件

### 更新
* 视图中的行和基础表中的行之间必须存在一对一的关系，以下情况不可更新
    * 聚合函数或窗口函数
    * DISTINCT
    * GROUP BY
    * HAVING
    * UNION / UNION ALL

### 作用
* 简单
    * 简化SQL语句
* 安全
    * 用户只能查询和修改所见到的数据
* 数据独立
    * 屏蔽真实表结构的变化

## 存储过程
**事先经过编译并存储在数据库中的一段SQL语句的集合，用来简化操作，减少数据在数据库和应用服务器之间的传输** 

* create
**在命令行中，使用关键字delimiter指定SQL语句的结束符** 
```sql
CREATE PROCEDURE name([paramters])
BEGIN
    -- SQL语句
END:
```

* use
```sql
CALL name([paramters]);
```

* check
```sql
-- 查询指定数据库的存储过程及状态
SELECT * FROM INFOMATION_SCHEMA.ROUTINES WHERE ROUTIN)SCHEMA = 'xxx';
-- 查询指定存储过程的定义
SHOW CREATE PROCEDURE name;
```

* delete
```sql
DROP PROCEDURE [IF EXISTS] name;
```

### 变量
#### 系统变量
* 全局变量
* 会话变量
```sql
-- check
SHOW [SESSION | GLOBAL] VARIABLES;
SHOW [SESSION | GLOBAL] VARIABLES LIKE '...';
SELECT @@[SESSION | GLOBAL] variable_name;

-- set
SET [SESSION | GLOBAL] 系统变量名=值;
SET @@[SESSION | GLOBAL] 系统变量名=值;
```

#### 用户定义变量
* assignment
```sql
SET @var_name =expr[,@var_name=expr]...;
SET @var_name :=expr[,@var_name=expr]...;
SELECT @var_name :=expr[,@var_name=expr]...;
SELECT 字段名 INTO @var_name FROM 表名;
```
* use
```sql
SELECT @var_name;
```

#### 局部变量
**作用范围为BEGIN ... END块** 
    * statement
    ```sql
    DECLARE 变量名 变量类型[DEFAULT ...];
    ```
    * assignment
    ```sql
    SET 变量名=值;
    SET 变量名:=值;
    SELECT 字段名 INTO 变量名 FROM 表名...;
    ```

### 条件判断
#### IF
```sql
IF condition1 THEN
    ...
ELSE condition2 THEN
    ...
ELSE
    ...
END IF;
```

**paramters**
| type  | content            | remark  |
|-------|--------------------|---------|
| IN    | input value        | default |
| OUT   | output value       |         |
| INOUT | input/output value |         |
```sql
CREATE PROCEDURE name([IN/OUT/INOUT paramters_name paramters_type])
BEGIN
    -- SQL语句
END:
```
#### CASE
    * grammer1
    ```sql
    CASE case_value
        WHEN when_value1 THEN statement_list1
        [ WHEN when_value1 THEN statement_list1 ]
        [ ELSE statement_list ]
    END CASE;
    ```
    * grammer2
    ```sql
    CASE
        WHEN search_condition1 THEN statement_list1
        [ WHEN search_condition2 THEN statement_list1 ]
        [ ELSE statement_list ]
    END CASE;
    ```

#### WHILE
```sql
-- judge the condition first,if true then enter the loop
WHILE condition DO
    ...
END WHILE;
```

#### REPEAT
```sql
-- excute the logic first,then judge the condition,if false then enter the loop
REPEAT
    ...
    UNTIL contidion
END REPEAT;
```

#### LOOP
    * LEAVE label;
    > exit the pointed label loop
    * ITERATE label;
    > enter the next loop
```sql
[begin_label:] LOOP
    ...
END LOOP [end_label];
```

**example:** 
```sql
create procedure p(in n int)
begin
    declare total int default 0;
    sum:loop
        if n <=0 then
            leave sum;
        end if;
        if n % 2 = 1 then
            set n := n - 1;
            iterate sum;
        end if;
        set total := total + n;
        set n := n - 1;
    end loop sum;
    select total;
end;
```

#### CURSOR
**存储查询结果集的数据类型,在存储过程中使用游标对结果集进行循环的处理** 
    * statement
    ```sql
    DECLARE cursor_name CURSOR FOR SQL语句;
    ```
    * open
    ```sql
    OPEN cursor_name;
    ```
    * get
    ```sql
    FETCH cursor_name INTO variable_name;
    ```
    * close
    ```sql
    CLOSE cursor_name;
    ```

#### handler
**条件处理程序可以定义在流程控制结构执行过程中遇到问题时相应的处理步骤**
```sql
DECLARE handler_action HANDLER FOR condition_value,... statement(SQL语句);

handler_action
    CONTINUE:继续执行当前程序
    EXIT:终止执行当前程序

condition_value
    SQLSTATE sqlstate_value:状态码，如02000
    SQLWARNING:所有以01开头的SQLSTATE代码的简写
    NOT FOUND:所有以02开头的SQLSTATE代码的简写
    SQLEXCEPTION:所有没有被SQLWARNING或NOT FOUND捕获的SQLSTATE代码的简写
```

## 存储函数
* **有返回值的存储过程，参数只能是IN类型**  
```sql
CREATE FUNCTION function_name(arguments)
RETURNS type [characteristic ...]
BEGIN
    ---SQL语句
    RETURN ...;
END;

characteristic:
* DETERMINISTIC:相同的输入参数产生相同的结果
* NO SQL:不包含SQL语句
* READS SQL DATA:包含读取数据的语句，但不包含写入数据的语句
```

## 触发器
* **与表相关的数据库对象，在insert/update/delete之前或之后，触发并执行触发器定义中的SQL语句集合** 

* **目前只支持行极触发，不支持语句触发** 

| 触发器类型 | NEW and OLD                                          |
|------------|------------------------------------------------------|
| INSERT     | NEW表示将要或已经新增的数据                          |
| UPDATE     | OLD表示修改之前的数据，NEW表示将要或已经修改后的数据 |
| DELETE     | OLD表示将要或已经删除的数据                          |

* create
```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_naem FOR EACH ROW -- 行级触发器
BEGIN
    trigger_stmt;
END;
```

* check
```sql
SHOW TRIGGERS;
```

* delete
```sql
-- 如果没有指定scherma_name，默认为当前数据库
DROP TRIGGER [scherma_name] trigger_name;
```

## 锁
**计算机协调多个进程或线程并发访问某一资源的机制** 

### 全局锁
**对整个数据库实例枷锁，加锁后整个实例就处于只读状态** 
```sql
-- add global lock
FLUSH TABLES WITH READ LOCK;

--single-transaction 不加锁完成一致性备份
mysqldump [-h ip_name] -u name -p password table_name > table_name.bak

-- unlock
UNLOCK TABLES;
```

### 表级锁
* 表锁
    * 表共享读锁(read lock) -- 阻塞其他客户端的写操作

    * 表独占写锁(write lock) -- 阻塞其他客户端的读，写操作
```sql
-- lock
LOCK TABLES table_name READ/WRITE;
-- unlock
UNLOCK TABLES / disconnect the mysq server;
```

* 元数据锁(meta data lock, MDL)
> 系统自动控制，在访问一张表的时候自动加上，用于维护表元数据的一致性，避免DML与DDL冲突

```sql
-- check the meta data lock
SELECT object_type,object_scheme,object_name,lock_type,lock_duration from performance_schema.metadata_locks;
```

| SQL                                        | 锁类型                                  | 说明                                            |
|--------------------------------------------|-----------------------------------------|-------------------------------------------------|
| lock tables read/write                     | SHARED_READ_ONLY / SHARED_NO_READ_WRITE |                                                 |
| select,select ... lock in share mode       | SHARED_READ                             | 与SHARED_READ.SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| insert,update,delete,select ... for update | SHARED_WRITE                            | 与SHARED_READ.SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| alter update ...                           | EXCLUSIVE                               | 与其他的MDL都互斥                               |

* 意向锁
**为了避免DML执行时加的行锁与表锁的冲突，在InnoDB中引入意向锁，使得表锁不用检查每行属否加锁** 
    * 意向共享锁(IS)
    * 意向排他锁(IX)

```sql
-- check the lock
SELECT object_type,object_scheme,object_name,lock_type,lock_duration from performance_schema.data_locks;
```
### 行级锁
**通过对索引上的索引项加锁** 

* 行锁 -- 锁定单个行记录
**如果不通过索引条件进行检索数据，此时行锁升级为表锁**
* 间隙锁 -- 锁定索引记录间隙(不包含该记录)，确保索引记录间隙不变，防止其他事务在这个间隙insert，产生幻读
    *索引上的等值查询(唯一索引)
* 临键锁 -- 行锁和间隙锁组合，同时锁住数据和数据前的间隙Gap

## 架构
### 内存架构
#### Buffer Pool
> 缓冲池是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增删改查操作时，先操作缓冲池中的数据(若缓冲池没有数据，则从磁盘加载并缓存),然后再以一定频率刷新到磁盘，从而减少磁盘IO,加快处理速度
**缓冲池一页为单位，底层采用链表数据结构管理** 
    * free page:空闲page
    * clean page:被使用page,数据没有被修改过
    * dirty page:脏页，被使用page,数据被修改过

#### Change Buffer
> 更改缓存区(针对非唯一二级索引页)，在执行DML语句时，如果这些数据page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更储存在Change Buffer中，在未来在未来数据被读取时，在将数据合并恢复到Buffer Pool中，在将合并后的数据刷新到磁盘中

* Adaptive Hash Index
> 自适应hash索引，用于优化对Buffer Pool数据的查询，系统根据情况自动完成

#### Log Buffer
> 日志缓存区，保存要写入到磁盘中的log日志数据(redo log,undo log),默认大小为16MB,日志缓冲区的日志会定期刷新到磁盘中
**参数:** 
    * innodb_log_buffer_size:缓冲区大小
    * innodb_flush_log_at_trx_commit:日志刷新到磁盘时机
        * 0 -- 每秒将日志写入并刷新磁盘一次
        * 1 -- 日志在每次事务提交时写入并刷新磁盘
        * 2 -- 日志在每次事务提交后写入，并每秒刷新到磁盘一次

### 磁盘结构
#### System Tablespace
> Change Buffer的存储区域
**参数:innodb_data_file_path** 

#### File-Per-Table Tablespaces
> 每个表的文件表空间包含单个InnoDB表的数据和索引，并存储在文件系统的单个数据文件中
**参数:innodb_file_per_table** 

#### General Tablespaces
> 通用表空间
```sql
CREATE TABLESPACE xxxx ADD DATAFILE 'file_name' ENGINE = engine_name;
CREATE TABLE xxx ... TABLESPACE ts_name;
```

#### Undo Tablespaces
> 撤销表空间，MySQL实例创建时默认创建两个undo表空间(初始大小为16M)，存储undo log日志

#### Temporary Tablespaces
> InnoDB使用会话临时表空间和全局临时表空间，存储用户创建的临时表等数据

#### Doublewrite Buffer Files
> 双写缓冲区，InnoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据也写入双写缓冲区文件中，便于系统异常时恢复数据

#### Redo Log
> 重做日志，用来实现事务的持久性，包含日志缓冲区(redo log buffer)和重做日志文件(redo log),前者在内存中，后者在磁盘中，当事务提交后会把所有修改信息存到该日志中


### 后台线程
#### Master Thread
> 核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中，保持数据的一致性，还包括脏页的刷新，合并插入缓存，undo页的回收

#### IO Thread
| 线程类型             | 默认个数 | 职责                     |
| Read thread          | 4        | read                     |
| Write thread         | 4        | write                    |
| Log thread           | 1        | 将日志缓冲区刷新到磁盘   |
| Insert buffer thread | 1        | 将写缓冲区内容刷新到磁盘 |

#### Purge Thread
> 回收事务已经提交了的undo log,在事务提交之后，undo log可能不用了，就用它来回收

#### Page Cleaner Thread
> 协助Master Thread刷新脏页到磁盘的线程，减轻Master Thread的压力


## 事务原理
### redo log
> 重做日志，记录事务提交时数据也的物理修改，实现事务的**持久性**

### undo log
> 记录数据被修改前的信息，用于回滚和MVCC(多版本并发控制),实现事务的**原子性**
* 销毁
    * 在事务执行时产生，事务提交时，并不会立即删除undo log,因为这些日志可能还用于MVCC
* 存储
    * 采用段的方式进行管理和记录

### undo log + redo log
> 实现事务的**一致性**

### 锁 + MVCC
> 实现事务的隔离性

## MVCC(Multi-Version Concurrency Control)
* 当前读
> 读取的是记录的最新版本，读取时保证其他并发事务不能修改该记录，会对读取的数据进行加锁

* 快照读
> 简单的select(不枷锁)，读取的是记录数据的可见版本，可能是历史数据
* Read Committed
    * 每次select都生成一个快照读
* Repeatable Read
    * 开启事务后第一个select语句才是快照读的地方
* Serializable
    * 快照读退化为当前读

### 隐藏字段
| 隐藏字段    | 含义                                                                |
| DB_TRX_ID   | 最近修改事务ID,记录插入这条记录或最后一次修改该记录的事务ID         |
| DB_ROLL_PTR | 回滚指针，指向这条记录的上一个版本，用于配合undo log,指向上一个版本 |
| DB_ROW_ID   | 隐藏之间，如果表结构没有指定主键，将会生成该隐藏字段                |

### undo log版本链
> 不同事务或相同事务对用一条记录进行修改，会导致该记录的undo log生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录
* insert时，产生的undo log日志只在回滚时需要，事务提交后立即删除
* update,delete时，产生的undo log不仅在回滚时需要，在快照读时也需要，不会立即删除

### readview
> 快照读SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务id
| 字段           | 含义                          |
|----------------|-------------------------------|
| m_ids          | 当前活跃的事务ID集合          |
| min_trx_id     | 最小活跃事务ID                |
| max_trx_id     | 预分配事务ID,当前最大事务ID+1 |
| creator_trx_id | ReadView创建者的事务ID        |

**版本链数据访问规则** 
* trx_id == creator_trx_id ? **可以**访问该版本
    * 数据由当前事务更改
* trx_id < min_trx_id ? **可以**访问该版本
    * 数据已经提交
* trx_id > max_trx_id ? **不可以**访问该版本
    * 该事务在ReadView生成后才开启
* min_trx_id <= trx_id <= max_trx_id ? 如果trx_id不在m_ids中是**可以**访问该版本的
    * 数据已经提交

## 系统数据库
| DataBase           | Performance                                                                           |
| mysql              | 存储MySQL服务器正常运行所需要的各种信息(时区，主从，用户，权限等)                     |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库，表，字段类型及访问权限等            |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数       |
| sy                 | 包含了一系列方便DBA和开发人员利用performance_schema性能数据库进行性能调优和诊断的视图 |
