---
title: mysql必知必会笔记(1)
date: 2020-06-07
tags: 
 - mysql，数据库
categories:
 - 数据库
---

# mysql必知必会(1)

数据库定义:数据库是一个以某种有组织的方式存储的数据集合。

表:表是一种结构化的文件，可用来存储某种特定类型的数据。

模式:表具有一些特性，这些特性定义了数据在表中如何存储，如可以存
储什么样的数据，数据如何分解，各部分信息如何命名，等等。描述表
的这组信息就是所谓的模式，模式可以用来描述数据库中特定的表以及
整个数据库（和其中表的关系）。

列:表中的一个字段。所有表都是由一个或多个列组
成的。

行:表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

主键:表中每一行都应该有可以唯一标识自己的一列（或一组列）。

主键是用来标识出唯一的行，如果没有主键，那么涉及到相关操作的时候，就有可能无法保证删除到正确的行。



## 主键设置规则

 任意两行都不具有相同的主键值；   
 每个行都必须具有一个主键值（主键列不允许NULL值）    
 不更新主键列中的值；    
 不重用主键列的值；    
 不在主键列中使用可能会更改的值。（例如，如果使用一个
名字作为主键以标识某个供应商，当该供应商合并和更改其
名字时，必须更改这个主键。）

## MySQL

数据的所有存储、
检索、管理和处理实际上是由数据库软件——DBMS（数据库管理系统）
完成的。MySQL是一种DBMS，即它是一种数据库软件。注意DBMS和数据库的概念完全不同

## 有用的命令行

SHOW COLUMNS FROM `表名` 列出表里所有的列  做代码生成器的时候可以用到    
DESCRIBE `表名` 列出表里所有的列  同上   
SHOW STATUS，用于显示广泛的服务器状态信息；   
SHOW CREATE DATABASE和SHOW CREATE TABLE，分别用来显示创建特定数据库或表的MySQL语句；   
SHOW GRANTS，用来显示授予用户（所有用户或特定用户）的安
全权限；   
SHOW ERRORS和SHOW WARNINGS，用来显示服务器错误或警告消息。

## 查询
- DISTINCT 排重关键字
  ```sql
  SELECT DISTINCT vend_idF FROM products
  SELECT DISTINCT vend_id,prod_price FROM `products`
  ```
  注意这个DISTINCT关键字作用与后面所有的列，后面跟着的所有列都一样才合并为一行，否在将分开显示

- limit 限制返回行数
  ```sql
  select prod_name from products limit 5,5; #从第 6 行开始，检索 5 行
  select prod_name from products limit 4 OFFSET 3; #从第 3 行开始，检索 4 行
  ```
  注意从mysql5.5之后新增了offset字段，目的是增强语义化。还有行数是从0开始的，limit5取得是第六行

- 使用完全限定的表名
  ```sql
  select products.prod_name from crashcourse.products;
  ```
- 排序 
  使用order by进行排序，默认asc升序排序，desc降序排序
  ```sql
  select prod_id, prod_price,prod_name 
  from products 
  order by prod_price desc, prod_name desc; #先按价格降序排列，再按产品名降序排列
  ```
  查询价格最高的商品(如果价格一样就有问题)，这个句子展示了limit必须在order by后面
  ```sql
  select prod_price from products order by prod_price desc limit 1; # 最高值 
  ```
- where 过滤数据   
  使用where字段过滤数据，除了常见的判断符，还有<>符号，表示不等
  ```sql
  select vend_id,prod_name from products where vend_id <> 1003; # 检索不是由1003供应商制造的所有产品 
  ```
  还有between范围值检查和null字符检查，这两个都用的少,注意这个is null，在数据库中null和''和0是由区分的，查询is null 那一列必须是null值
  ```sql
  select prod_name,prod_price from products where prod_price between 5 and 10; # 价格 大于等于5，小于等于10 的产品名、产品价格
  ```
  ```sql
  select prod_name from products where prod_price is null;  # 返回prod_price为空值null的prod_name,无对应数据 
  ```
-
