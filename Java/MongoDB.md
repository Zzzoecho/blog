# MongoDB

mars: 10.1.5.188



## 概念

collection - 表, table

documnet - 行, row

field - 数据字段/域, column



## 常用命令



### 数据库

```shell
show dbs // 显示所有数据库列表

db // 显示当前使用的表

use xxx // 连接到指定数据库, 不存在则创建 (但只有插入内容后才算真正创建)

db.dropDatabase() // 删除数据库


```



### 集合

```shell
db.createCollection("name", options)

db.COLLECTION_NAME.insert(document)

db.collection.drop()

show collections
```