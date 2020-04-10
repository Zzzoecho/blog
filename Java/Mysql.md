# Mysql

公司: 测试库 10.1.4.11

1. 进入 mysql `mysql -uroot` 
2. 本地 `mycli -u[user] -h[server] -p[password] `

## 基本命令

### 数据库方面

- 展示 `show databses;` 注意复数
- 创建 `create database <name>;`
- 删除 `drop database <name>;`
- 选择 `use <name>;`

### 表

- 创建 `create table <tableName> (<columnType> <columnType>);`
- 修改表名或字段 `alert`

```sql
-- 创建
CREATE TABLE IF NOT EXISTS `table`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `title` VARCHAR(100) NOT NULL [comment],
   `author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 编辑表
ALTER TABLE tableName RENAME TO newname; -- 修改表名
ALTER TABLE tableName DROP <column>; -- 删除列
ALTER TABLE tableName ADD <column> type [插入位置]; -- 新增列
ALTER TABLE tableName MODIFY <column> newtype [还可以指定默认值]; -- 修改列类型
ALTER TABLE tableName CHANGE <column> newName newtype not null default 10; -- 修改列名和类型
ALTER TABLE tableName ALTER <column> SET DEFAULT 1000; -- 修改列的默认值
```

创建可用关键字
> AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
> default 默认值
> comment 注释
> PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。
> ENGINE 设置存储引擎，CHARSET 设置编码。

编辑可用关键字
> 新增列的插入位置: `first` -  第一列, `after 列名` - 放到某一列之后

---

表的增删改查

- 插入 `insert into [tableName] ([columnName], ...) values ([columnValue], ...);`
- 查询 `select columnName from [tableName]  [查询条件];`
- 更新 `update [tableName] set [key=new value] [where]`
- 删除 `delete from [tableName] [where];`

> columnName, 可传多个, * 表全部
> tableName, 可传多个

## 查询语句

where: 定位, 多条件 可用 `and` 和 `or`
REGEXP: 正则
like: 模糊匹配 `%com` 搜索含 com 的所有
union: 连接多次查询的结果, 默认去重. union all: 不去重
order by: 排序,`order by [column] [asc|desc]` 默认升序(asc)
group by: 将数据用函数处理后分组, 常用函数: COUNT, SUM, AVG
with rollup: 结尾再加一行, 用同样的函数计算

```sql
select column, function(args) from tableName group by columnName;

SELECT name, SUM(singin) as '共计' FROM table GROUP BY name WITH ROLLUP;
// SUM(singin) as '共计': 给列重命名为 '共计', 使用 SUM 函数计算 singin 的总和
```

join
> inner join: 交集
> left join: 左表中所有数据
> right join: 右表中所有数据

```sql
SELECT a.id, a.author, b.count FROM tbl a INNER JOIN count_tbl b ON a.author = b.author;
```

## Null 值处理

运算符: `is null`, `is not null`, `<=>` 当比较的两个值相等或都是 null 时返回 true

mysql 中, null 与其他任何值(包括 null) 的比较永远返回 null

## 事务

> 事务(transaction)主要用于处理操作量大, 复杂度高的数据

必须满足4个条件(ACID):

1. 原子性(Atomicity): 一个事务里的所有操作, 要么全部完成, 要么全部不完成. 如果在执行过程中出错, 会回滚到事务开始前的状态
2. 一致性(Consistency): 事务开始前和结束后, 数据库的完整性没有被破坏
3. 隔离性(Isolation): 数据库允许多个并发事务同时对数据库进行读写和修改的能力. 事务隔离分不同级别, 读未提交(read uncommitted), 读提交(read committed), 可重复读(repeatable read)和串行化(serializable)
4. 持久性(Durability): 事务处理结束后, 对数据的修改是永久的

### 事务控制语句

- BEGIN 或 START TRANSACTION 显式地开启一个事务；
- COMMIT 也可以使用 COMMIT WORK，不过二者是等价的。COMMIT 会提交事务，并使已对数据库进行的所有修改成为永久性的；
- ROLLBACK 也可以使用 ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
- SAVEPOINT identifier，SAVEPOINT 允许在事务中创建一个保存点，一个事务中可以有多个 SAVEPOINT；
- RELEASE SAVEPOINT identifier 删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；
- ROLLBACK TO identifier 把事务回滚到标记点；
- SET TRANSACTION 用来设置事务的隔离级别。InnoDB 存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ 和 SERIALIZABLE。

基本流程

1. begin 开始一个事务, 类似开个新分支
2. 经过一系列漫长复杂的操作
3. 有问题 -> 回滚; 没问题 -> commit

## 索引

单列索引: 一个索引只包含单个列, 一个表可以有多个单列索引
组合索引: 一个索引包含多个列

## 数据类型

### 数值

|类型|用途|
|---|---|
|tinyint|小整数|
|smallint|大整数|
|mediumint|大整数|
|int|大整数|
|bigint|极大整数|
|float|单精度浮点数|
|double|双精度浮点数|
|decimal|小数|

### 日期和时间

|类型|格式|用途|
|---|---|---|
|date|YYYY-MM-DD|日期|
|time|HH:MM:SS|时间|
|year|YYYY|年份|
|datetime|YYYY-MM-DD HH:MM:SS|日期+时间|
|timestamp|YYYYMMDD HHMMSS|时间戳|

### 字符串类型

|类型|用途|
|---|---|
|char|定长字符串|
|varchar|变长字符串|
|tinyblob|不超过255个字符的二进制字符串|
|tinytext|短文本字符串|
|blob|二进制数据|
|text|长文本数据|
|mediumblob|中等长度的二进制数据|
|mediumtext|中等长度的文本数据|
|longblob|极大二进制数据|
|longtext|极大文本数据|
