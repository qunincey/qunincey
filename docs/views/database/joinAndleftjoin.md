---
title: join和left join的区别
date: 2020-06-29
tags: 
 - mysql，数据库
categories:
 - 数据库
---

## left join 和 right join
这两个的区别在于返回的列以谁的为准，left join就以左边表为准，right join就以右边的表为准

## join 和 inner join
join等价于inner join，inner join和上面两个join1的区别在于 inner join 返回两张表都
有的列。

## on关键字
on关键字在联结表时起过滤的作用，注意的是，如果是内联结，那么这个on和where起的作用是一样的，这个on关键字是专门为外联结准备的，当外联结的其中一张表没有匹配到的时候，根据on字段判断是否显示在结果集里。

## 自联结
mysql还支持表联结自己
```sql
select * from s1 join s1
```

## 总结
join和inner join都被称为内联结，使用内联结的时候，例如t1 join t2，如果t2里面是空的，那么这条数据就不会显示，所以才会产生外联结，也就left join 和 right join，这两种join都会以其中一张表为根据，如果列为空，那么就会自动填充null值显示。