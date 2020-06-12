---
title: mysql必知必会笔记(1)
date: 2020-06-07
tags: 
 - mysql，数据库
categories:
 - 数据库
---

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
- and 和 or   
  使用and和or进行where子句连接，但要注意，同时使用and，or时，查询是由顺序的，必须使用()将子句包裹起来。
  ```sql
  select prod_name,prod_price from products where vend_id = 1002 or vend_id = 1003; 
  ```
- in 和 not   
  in
  ```sql
  select prod_name,prod_price from products
  where vend_id in (1002,1003) order by prod_name;
  ```
  not
  ```sql
  select prod_name,prod_price from products
  where vend_id not in (1002,1003) order by prod_name;
  # Mysql支持not对in，between，exsits子句取反 
  ```
# 搜索

- 通配符搜索    
  使用%和_进行通配符搜索，%匹配多个字符，_匹配一个字符,使用like匹配字段中包含anvil，并且anvil出现的次数不受限制。
  ```sql
  select prod_id,prod_name from products where prod_name like "%anvil%"; 
  ```
  注意 % 不能匹配null值，并且将%放在句首将会拖慢搜索速度
- 正则表达式匹配
  mysql中也可以使用正则匹配，但是对于正则表达式的支持比较有限，这里只记录书中出现的

  在正则表达式中，匹配任意 一个 字符 
  ```sql
  select prod_name from products where prod_name regexp ".000";
  ```
  正则表达式匹配默认不分大小写，需使用BINARY区分大小写
  ```sql
  select prod_name from products where prod_name regexp binary "JetPack .000";
  ```
  正则表达式的OR操作符： |
  ```sql
  select prod_name from products where prod_name regexp "1000|2000" order by prod_name;
  ```
  正则表达式匹配几个字符之一 [ ]
  ```sql
  select prod_name from products where prod_name regexp '[123] Ton' order by prod_name;  # [123]匹配单一字符：1或2或3
  ```
  正则表达式匹配范围 
  ```sql
  select prod_name from products where prod_name regexp '[1-5] Ton' order by prod_name;  # [1-5]匹配1,2,3,4,5
  ```
  正则表达式匹配特殊字符，必须用\\前导，进行转义 
  多数正则使用单反斜杠转义，但mysql使用双反斜杠，mysql自己解释一个，正则表达式库解释一个
  ```sql
  select vend_name from vendors where vend_name regexp "\\." order by vend_name; # ‘\\.'匹配字符.
  ```
- 正则表达式匹配字符类    
  [:alnum:]	任意字母和数字（同[a-zA-Z0-9]） 
  [:alpha:]	任意字符（同[a-zA-Z]）    
  [:blank:]	空格和制表（同[\\t]）    
  [:cntrl:]	ASCII控制字符（ASCII 0到31和127）    
  [:digit:]	任意数字（同[0-9]）    
  [:graph:]	与[:print:]相同，但不包括空格 
  [:lower:]	任意小写字母（同[a-z]）    
  [:print:]	任意可打印字符 
  [:punct:]	既不在[:alnum:]又不在[:cntrl:]中的任意字符 
  [:space:]	包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]）    
  [:upper:]	任意大写字母（同[A-Z]）    
  [:xdigit:]	任意十六进制数字（同[a-fA-F0-9]）    
  ```sql
  select prod_name from products where prod_name regexp '[:digit:]' order by prod_name; #[:digit:]匹配任意数字 
  ```

- 匹配多个实例 
  mysql正则中对于多个重复字符的支持   
  ![重复字符](../database/image/bjbh01.png)
  ```sql
  select prod_name from products where prod_name regexp '\\([0-9] sticks?\\)'
  order by prod_name;  # 返回了'TNT (1 stick)'和'TNT (5 sticks)'
  ```
- 定位符      
  !['定位符'](../database/image/bibh02.png)
  ```sql
  select prod_name from products where prod_name regexp '^[0-9\\.]' order by prod_name; #找出以一个数（包括以小数点开始的数）开始的所有产品
  select prod_name from products where prod_name regexp '[0-9\\.]' order by prod_name;  #找出包括小数点和数字的所有产品
  ```
# 函数
- 拼接字段 concat()
  ```sql
  select concat(vend_name,' (',vend_country,')') from vendors order by vend_name; 
  ```
- 删除空格   
  删除数据左侧多余空格 ltrim()   
  删除数据两侧多余空格 trim()   
  删除数据右侧多余空格 rtrim()  
  ```sql
  select concat(rtrim(vend_name),' (',rtrim(vend_country),')') from vendors order by vend_name;
  ``` 

- as赋予别名
  ```sql
  select concat(rtrim(vend_name),' (',rtrim(vend_country),')') as vend_title from vendors order by vend_name;
  ```

- 执行算数计算    
  ```sql
  select prod_id,quantity,item_price from orderitems where order_num = 20005;
  ```

- 文本函数     
  
  ![alt](../database/image/bjbh03.png)
  ![alt](../database/image/bjbh04.png)
  ```sql
  select vend_name, upper(vend_name) as vend_name_upcase from vendors order by vend_name;
  ```
  soundex() 描述语音表示的字母数字模式的算法,对串按照发音比较而不是字母比较
  ```sql
  select cust_name,cust_contact from customers where cust_contact = 'Y. Lie';  # 无返回 
  select cust_name,cust_contact from customers where soundex(cust_contact) = soundex('Y. Lie'); # 按发音搜索 
  ```

- 日期函数    
  常用日期函数
  ![alt](../database/image/bjbh05.png)     
  按照date()日期进行过滤信息，更可靠 
  ```sql
  select cust_id,order_num from orders where date(order_date) = "2005-09-01";
  ```
- 聚合函数   
  ![alt](../database/image/bjbh07.png)
  avg() 求和，忽略null
  ```sql
  select avg(prod_price) as avg_price from products;
  ```
  count() 对行的数目进行统计 忽略null
  ```sql
  select count(*) as num_cust from customers; 
  ```
  max() & min()  最大最小
  ```sql
  select max(prod_price) as max_price from products;
  select min(prod_price) as min_price from products;
  ```
  在用于文本数据时，如果数据按相应的列排序，则MIN()返回最前面一行
  ```sql
  select min(prod_name) from products; 
  ```
  sum 求和
  ```sql
  select sum(quantity) as items_ordered from orderitems;
  ```
  使用聚合函数时去重
  ```sql
  select avg(distinct prod_price) as avg_price from products where vend_id = 1003;
  ```
  在使用COUNT的使用指定条件，最后必须加上NULL，因为COUNT是不计算null的，如果这行数据的release_year不等于2006，那么就是返回null，而null不被计算，所以用这种方式进行筛选
  ```sql
  count(release_year = '2006' or NULL) 
  ```



 

  
