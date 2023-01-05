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

# multi-table query
* one-to-many
> 多的方面构建外键
* many to many
> 建立第三张中间表，中间表至少包含两个外键，分别关联两方主键
* one-to-one
> 用于单表拆分，基础字段和其他详细字段分开存放
* query
    * 连接查询
        * 内连接：查询交集部分数据
        ```sql
        -- 隐式内连接
        SELECT 字段列表 FROM 表1,表2 WHERE 条件 ...;
        -- 显式内连接
        SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件 ...;
        ```
        * 外连接：
            * 左外连接：查询左表所有数据，以及两张表交集部分的数据
            ```sql
            SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
            ```
            * 右外连接：查询右表所有数据，以及两张表交集部分的数据
            ```sql
            SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
            ```
        * 自连接：当前表与自身的连接查询，自连接必须使用表别名
            ```sql
            SELECT 字段列表 FROM 表1 别名1 JOIN 表2 别名2 ON 条件...;
            ```
    * 联合查询(字段列表需完全相同)
    ```sql
    SELECT 字段列表 FROM 表1...
    UNION [ALL] --- 去掉ALL实现去重
    SELECT 字段列表 FROM 表1...;
    ```
    * 子查询(嵌套SELECT语句)
    ```sql
    SELECT 字段列表 FROM 表1 WHERE 字段1 = (SELECT 字段1 FROM 表2);
    ```

    > 标量子查询(子查询结果为单个值)

    常用的操作符: =, <>, >, >=, <, <=
    > 列子查询(子查询结果为一列)

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
* 客户端
* 服务端
    * 连接层
    * 服务层
    * 引擎层
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
        2. Segment:段
        3.Extent(1M):区
        4.Page(16K):页
        5.Row:行
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

    * 存储层

## 索引(index)
**帮助MySQL高效获取数据的数据结构(有序)** 
| 索引结构            | 描述                           |
| B+Tree索引          | 最常见的索引类型               |
| Hash索引            | 精确匹配索引，不支持范围查询   |
| R-tree(空间索引)    | MyISAM引擎的一个特殊索引类型   |
| Full-text(全文索引) | 通过建立倒排索引，快速匹配文档 |

* B-Tree(多路平衡查找树)
```text
以一颗最大度数(子节点个数)为5的b-tree为例(每个节点最多存储4个key,5个指针)
```
* B+Tree(MySQL)
    * 所有的数据都会出现在叶子节点
    * 叶子节点构成一个双向链表(原本为单向链表)
* Hash
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
